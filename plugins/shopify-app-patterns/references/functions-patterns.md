# Shopify Functions Patterns

## Table of Contents
1. [How Functions Work](#how-functions-work)
2. [Function Anatomy](#function-anatomy)
3. [Discount Functions](#discount-functions)
4. [Payment Customization Functions](#payment-customization)
5. [Delivery Customization Functions](#delivery-customization)
6. [Cart Validation Functions](#cart-validation)
7. [Order Routing Functions](#order-routing)
8. [Testing Functions](#testing-functions)
9. [Performance Considerations](#performance)

---

## How Functions Work

Shopify Functions inject custom backend logic into Shopify's execution pipeline. They compile to WebAssembly and run inside Shopify's infrastructure — you never host them.

**Execution flow:**
1. Shopify encounters an extension target (e.g., discount calculation during checkout)
2. Shopify runs your function's **input query** (GraphQL) to gather the data your function needs
3. The query result (JSON) is passed as stdin to your Wasm module
4. Your function processes the input and writes JSON output to stdout
5. Shopify applies the operations described in your output

**Key constraint**: Functions are stateless and have no network access. All data must come through the input query. If you need external data, store it in metafields and query them in the input.

---

## Function Anatomy

### Directory structure

```
extensions/my-function/
├── shopify.extension.toml    # Configuration + input query
├── src/
│   └── run.js                # (or run.rs for Rust)
├── package.json
└── input.graphql              # Input query (optional, can be inline in TOML)
```

### Configuration

```toml
api_version = "2026-01"

[[extensions]]
name = "Volume Discount"
handle = "volume-discount"
type = "function"

[extensions.build]
command = "npm exec -- shopify app function build"
path = "dist/function.wasm"

[[extensions.targeting]]
target = "purchase.discount.run"
input_query = "src/run.graphql"
export = "run"

[extensions.ui]
handle = "volume-discount-settings"

[extensions.ui.paths]
create = "/app/discount/new"
details = "/app/discount/:functionId"
```

### Input query

The input query defines what data your function receives. It runs against a special Functions API schema (not the full Admin API).

```graphql
# src/run.graphql
query RunInput {
  cart {
    lines {
      quantity
      merchandise {
        ... on ProductVariant {
          id
          product {
            id
            hasAnyTag(tags: ["vip-eligible"])
          }
        }
      }
      cost {
        amountPerQuantity {
          amount
          currencyCode
        }
      }
    }
  }
  discountNode {
    metafield(namespace: "$app", key: "config") {
      value
    }
  }
}
```

### Function logic (JavaScript)

```javascript
// src/run.js
// @ts-check

/**
 * @typedef {import("../generated/api").RunInput} RunInput
 * @typedef {import("../generated/api").FunctionRunResult} FunctionRunResult
 */

/**
 * @param {RunInput} input
 * @returns {FunctionRunResult}
 */
export function run(input) {
  const config = JSON.parse(
    input?.discountNode?.metafield?.value ?? "{}"
  );

  const { minQuantity = 3, percentage = 10 } = config;

  const targets = input.cart.lines
    .filter(line => line.quantity >= minQuantity)
    .map(line => ({
      productVariant: { id: line.merchandise.id }
    }));

  if (targets.length === 0) {
    return { discounts: [] };
  }

  return {
    discounts: [
      {
        message: `${percentage}% volume discount`,
        targets,
        value: {
          percentage: { value: percentage.toString() }
        },
      },
    ],
  };
}
```

### Function logic (Rust — recommended for performance)

```rust
use shopify_function::prelude::*;
use shopify_function::Result;

#[shopify_function_target(query_path = "src/run.graphql", schema_path = "schema.graphql")]
fn run(input: input::ResponseData) -> Result<output::FunctionRunResult> {
    let config: serde_json::Value = input
        .discount_node
        .metafield
        .as_ref()
        .map(|m| serde_json::from_str(&m.value).unwrap())
        .unwrap_or(serde_json::json!({}));

    let min_quantity = config["minQuantity"].as_i64().unwrap_or(3);
    let percentage = config["percentage"].as_f64().unwrap_or(10.0);

    let targets: Vec<_> = input
        .cart
        .lines
        .iter()
        .filter(|line| line.quantity >= min_quantity)
        .filter_map(|line| {
            if let input::InputCartLinesMerchandise::ProductVariant(variant) = &line.merchandise {
                Some(output::Target {
                    product_variant: Some(output::ProductVariantTarget {
                        id: variant.id.clone(),
                    }),
                })
            } else {
                None
            }
        })
        .collect();

    if targets.is_empty() {
        return Ok(output::FunctionRunResult { discounts: vec![] });
    }

    Ok(output::FunctionRunResult {
        discounts: vec![output::Discount {
            message: Some(format!("{}% volume discount", percentage)),
            targets,
            value: output::Value::Percentage(output::Percentage {
                value: percentage.to_string(),
            }),
        }],
    })
}
```

---

## Discount Functions

Target: `purchase.discount.run`

Discount functions define new discount types that merchants can create and manage through the Shopify admin. They replace Shopify Scripts for discount logic.

### Output structure

```json
{
  "discounts": [
    {
      "message": "Volume discount: 10% off",
      "targets": [
        { "productVariant": { "id": "gid://shopify/ProductVariant/123" } }
      ],
      "value": {
        "percentage": { "value": "10.0" }
      }
    }
  ]
}
```

### Value types

```json
// Percentage discount
{ "percentage": { "value": "15.0" } }

// Fixed amount per item
{ "fixedAmount": { "amount": "5.00" } }
```

### Merchant configuration UI

Functions don't have their own UI — you build the configuration page in your app home:

```toml
[extensions.ui]
handle = "discount-settings"

[extensions.ui.paths]
create = "/app/discount/new"        # Shown when merchant creates a new discount
details = "/app/discount/:functionId" # Shown when merchant edits the discount
```

Your React Router app renders these pages, where merchants configure the discount parameters. Store the configuration as a metafield on the discount node.

---

## Payment Customization Functions

Target: `purchase.payment-customization.run`

Hide, reorder, or rename payment methods at checkout.

### Output structure

```json
{
  "operations": [
    {
      "hide": {
        "paymentMethodId": "gid://shopify/PaymentCustomizationPaymentMethod/1"
      }
    },
    {
      "rename": {
        "paymentMethodId": "gid://shopify/PaymentCustomizationPaymentMethod/2",
        "name": "Pay on delivery (orders under $100)"
      }
    },
    {
      "move": {
        "paymentMethodId": "gid://shopify/PaymentCustomizationPaymentMethod/3",
        "index": 0
      }
    }
  ]
}
```

### Example: hide COD for high-value orders

```javascript
export function run(input) {
  const cartTotal = parseFloat(input.cart.cost.totalAmount.amount);

  if (cartTotal <= 500) {
    return { operations: [] };
  }

  const codMethod = input.paymentMethods.find(
    m => m.name.includes("Cash on Delivery")
  );

  if (!codMethod) return { operations: [] };

  return {
    operations: [{ hide: { paymentMethodId: codMethod.id } }],
  };
}
```

---

## Delivery Customization Functions

Target: `purchase.delivery-customization.run`

Rename, reorder, or hide delivery options at checkout.

### Output structure

```json
{
  "operations": [
    {
      "rename": {
        "deliveryOptionHandle": "standard-shipping",
        "title": "Standard Shipping (3-5 business days)"
      }
    },
    {
      "hide": {
        "deliveryOptionHandle": "express-international"
      }
    }
  ]
}
```

---

## Cart Validation Functions

Target: `purchase.validation.run`

Block or warn customers during checkout based on custom rules.

### Output structure

```json
{
  "errors": [
    {
      "localizedMessage": "Maximum 5 items per product allowed",
      "target": "cart"
    }
  ]
}
```

### Example: quantity limit per variant

```javascript
export function run(input) {
  const MAX_QUANTITY = 5;
  const errors = [];

  for (const line of input.cart.lines) {
    if (line.quantity > MAX_QUANTITY) {
      errors.push({
        localizedMessage: `Maximum ${MAX_QUANTITY} units per item. "${line.merchandise.product.title}" has ${line.quantity}.`,
        target: "cart",
      });
    }
  }

  return { errors };
}
```

Empty `errors` array = validation passes, checkout proceeds.

---

## Order Routing Functions

### Location rules
Target: `purchase.order-routing.run`

Influence which fulfillment location Shopify selects for an order.

### Fulfillment constraints
Target: `purchase.fulfillment-constraint.run`

Define constraints on how orders can be fulfilled (e.g., "ship from same warehouse" or "don't split across locations").

---

## Testing Functions

### Local testing

```bash
# Generate sample input from your input query
shopify app function typegen

# Run the function locally with sample input
echo '{"cart": {...}}' | shopify app function run --path extensions/my-function
```

### Unit testing (JavaScript)

```javascript
import { describe, it, expect } from "vitest";
import { run } from "./run";

describe("volume discount", () => {
  it("applies discount when quantity >= 3", () => {
    const input = {
      cart: {
        lines: [
          {
            quantity: 5,
            merchandise: { id: "gid://shopify/ProductVariant/1" },
            cost: { amountPerQuantity: { amount: "10.00" } },
          },
        ],
      },
      discountNode: {
        metafield: {
          value: JSON.stringify({ minQuantity: 3, percentage: 10 }),
        },
      },
    };

    const result = run(input);

    expect(result.discounts).toHaveLength(1);
    expect(result.discounts[0].value.percentage.value).toBe("10");
  });

  it("returns no discounts when quantity < minimum", () => {
    const input = {
      cart: {
        lines: [
          {
            quantity: 1,
            merchandise: { id: "gid://shopify/ProductVariant/1" },
            cost: { amountPerQuantity: { amount: "10.00" } },
          },
        ],
      },
      discountNode: {
        metafield: {
          value: JSON.stringify({ minQuantity: 3, percentage: 10 }),
        },
      },
    };

    const result = run(input);

    expect(result.discounts).toHaveLength(0);
  });
});
```

---

## Performance Considerations

**Shopify strongly recommends Rust over JavaScript for functions.** JavaScript functions can timeout with large carts (hundreds of line items). Rust compiles to much smaller, faster Wasm.

### Performance budget

Functions have strict execution limits:
- **Execution time**: Must complete within milliseconds
- **Memory**: Limited Wasm memory
- **Input size**: Grows with cart size — request only what you need in the input query

### Optimization strategies

1. **Minimize input query** — don't fetch fields you won't use
2. **Avoid complex data structures** — flat loops beat nested object graphs
3. **Short-circuit early** — return empty results as soon as you know the function doesn't apply
4. **Pre-compute** — store computed values in metafields rather than calculating at runtime
5. **Use `hasAnyTag` / `inAnyCollection`** — these are optimized server-side filters in the input query, much faster than fetching all tags and filtering in your function

```graphql
# Good: server-side filter
product {
  hasAnyTag(tags: ["sale", "clearance"])
  inAnyCollection(ids: ["gid://shopify/Collection/1"])
}

# Bad: fetch all and filter client-side
product {
  tags
  collections(first: 50) { edges { node { id } } }
}
```
