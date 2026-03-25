# GraphQL Patterns for Shopify Apps

## Table of Contents
1. [Query Structure](#query-structure)
2. [Mutations](#mutations)
3. [Pagination](#pagination)
4. [Bulk Operations](#bulk-operations)
5. [Rate Limits & Cost Management](#rate-limits)
6. [Error Handling](#error-handling)
7. [Direct API Access from Extensions](#direct-api-access)
8. [Common Query Patterns](#common-patterns)

---

## Query Structure

Shopify's GraphQL Admin API uses a single endpoint. Every query starts from the `QueryRoot`.

```graphql
# Fetch a product with specific fields only
query getProduct($id: ID!) {
  product(id: $id) {
    id
    title
    handle
    status
    totalInventory
    priceRangeV2 {
      minVariantPrice {
        amount
        currencyCode
      }
    }
  }
}
```

### Connections (related data in one query)

GraphQL's power is fetching related data in a single request. Where REST requires multiple calls, GraphQL uses connections:

```graphql
query orderWithDetails($id: ID!) {
  order(id: $id) {
    id
    name
    totalPriceSet {
      shopMoney { amount currencyCode }
    }
    customer {
      displayName
      email
      metafields(first: 5) {
        edges {
          node { namespace key value }
        }
      }
    }
    lineItems(first: 50) {
      edges {
        node {
          title
          quantity
          variant {
            sku
            product {
              handle
              productType
            }
          }
        }
      }
    }
  }
}
```

### Variables

Always use variables instead of string interpolation — prevents injection and enables query caching:

```typescript
const response = await admin.graphql(`
  query getProduct($id: ID!) {
    product(id: $id) {
      title
      status
    }
  }
`, {
  variables: { id: "gid://shopify/Product/123456789" }
});
```

### Aliases

Fetch the same field multiple times with different arguments:

```graphql
query {
  activeProducts: products(first: 5, query: "status:active") {
    edges { node { id title } }
  }
  draftProducts: products(first: 5, query: "status:draft") {
    edges { node { id title } }
  }
}
```

### Fragments

Reuse field selections across queries:

```graphql
fragment ProductFields on Product {
  id
  title
  handle
  status
  totalInventory
}

query {
  product(id: "gid://shopify/Product/123") {
    ...ProductFields
  }
  products(first: 10) {
    edges {
      node { ...ProductFields }
    }
  }
}
```

---

## Mutations

Mutations modify data. Always request `userErrors` — this is where validation failures are reported.

```graphql
mutation createProduct($input: ProductInput!) {
  productCreate(input: $input) {
    product {
      id
      title
      handle
    }
    userErrors {
      field
      message
    }
  }
}
```

Variables:
```json
{
  "input": {
    "title": "New Product",
    "productType": "Accessories",
    "vendor": "My Brand",
    "tags": ["new", "featured"]
  }
}
```

### Handling mutation responses

```typescript
const response = await admin.graphql(MUTATION, { variables: { input } });
const json = await response.json();

if (json.data.productCreate.userErrors.length > 0) {
  // Validation failed — show errors to user
  const errors = json.data.productCreate.userErrors;
  throw new Error(errors.map(e => `${e.field}: ${e.message}`).join(", "));
}

const product = json.data.productCreate.product;
```

### Metafields in mutations

Attach metafields directly when creating/updating resources:

```graphql
mutation updateProduct($input: ProductInput!) {
  productUpdate(input: $input) {
    product {
      id
      metafield(namespace: "$app", key: "rating") {
        value
      }
    }
    userErrors { field message }
  }
}
```

Variables:
```json
{
  "input": {
    "id": "gid://shopify/Product/123",
    "metafields": [
      {
        "namespace": "$app",
        "key": "rating",
        "value": "4.5",
        "type": "number_decimal"
      }
    ]
  }
}
```

---

## Pagination

Shopify uses Relay-style cursor pagination. Never use offset-based pagination.

### Forward pagination

```graphql
query products($first: Int!, $after: String) {
  products(first: $first, after: $after) {
    edges {
      cursor
      node {
        id
        title
      }
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

### Pagination loop in code

```typescript
let hasNextPage = true;
let cursor = null;
const allProducts = [];

while (hasNextPage) {
  const response = await admin.graphql(PRODUCTS_QUERY, {
    variables: { first: 250, after: cursor }
  });
  const json = await response.json();
  const { edges, pageInfo } = json.data.products;

  allProducts.push(...edges.map(e => e.node));
  hasNextPage = pageInfo.hasNextPage;
  cursor = pageInfo.endCursor;
}
```

For datasets larger than a few thousand records, use bulk operations instead.

---

## Bulk Operations

For large data exports/imports, use the bulk operation API. It runs asynchronously and writes results to a JSONL file.

### Bulk query

```graphql
mutation {
  bulkOperationRunQuery(
    query: """
    {
      products {
        edges {
          node {
            id
            title
            variants {
              edges {
                node {
                  id
                  sku
                  price
                }
              }
            }
          }
        }
      }
    }
    """
  ) {
    bulkOperation {
      id
      status
    }
    userErrors { field message }
  }
}
```

### Poll for completion

```graphql
query {
  currentBulkOperation {
    id
    status
    errorCode
    createdAt
    objectCount
    fileSize
    url  # JSONL download URL when completed
    partialDataUrl
  }
}
```

Or subscribe to the `bulk_operations/finish` webhook for async notification.

### Process JSONL results

Each line is a JSON object. Parent-child relationships use `__parentId`:

```jsonl
{"id":"gid://shopify/Product/1","title":"Widget"}
{"id":"gid://shopify/ProductVariant/1","sku":"WDG-001","price":"29.99","__parentId":"gid://shopify/Product/1"}
```

---

## Rate Limits

GraphQL uses **calculated query cost**, not request count.

- **Budget**: 1000 cost points maximum per query
- **Restore rate**: 50 points per second
- **Bucket maximum**: 1000 points (stores up to 20 seconds of unused capacity)

### Check your cost

Every response includes cost info in `extensions`:

```json
{
  "extensions": {
    "cost": {
      "requestedQueryCost": 42,
      "actualQueryCost": 42,
      "throttleStatus": {
        "maximumAvailable": 1000.0,
        "currentlyAvailable": 958,
        "restoreRate": 50.0
      }
    }
  }
}
```

### Cost reduction strategies

1. Request only the fields you need
2. Reduce `first`/`last` arguments (fetching 250 items costs more than fetching 10)
3. Use bulk operations for large datasets
4. Avoid deeply nested connections

If throttled, you get `MAX_COST_EXCEEDED` or `THROTTLED` errors with HTTP 200.

---

## Error Handling

### Response structure

GraphQL can return partial data alongside errors:

```json
{
  "data": { "product": null },
  "errors": [
    {
      "message": "Product not found",
      "extensions": { "code": "NOT_FOUND" }
    }
  ]
}
```

### Error codes to handle

| Code | Meaning | Action |
|------|---------|--------|
| `THROTTLED` | Rate limited | Wait and retry with exponential backoff |
| `MAX_COST_EXCEEDED` | Query too expensive | Simplify query or use bulk operations |
| `INTERNAL_SERVER_ERROR` | Shopify issue | Retry with backoff; check shopifystatus.com |
| `ACCESS_DENIED` | Missing scopes | Check app scopes in shopify.app.toml |

### Defensive pattern

```typescript
async function shopifyQuery(admin, query, variables = {}) {
  const response = await admin.graphql(query, { variables });
  const json = await response.json();

  if (json.errors) {
    const throttled = json.errors.find(
      e => e.extensions?.code === "THROTTLED"
    );
    if (throttled) {
      await sleep(2000);
      return shopifyQuery(admin, query, variables); // retry
    }
    throw new ShopifyAPIError(json.errors);
  }

  return json.data;
}
```

---

## Direct API Access from Extensions

UI extensions can call the Admin GraphQL API directly without a server, using the `shopify:admin` protocol:

```javascript
// Inside a UI extension (admin action, admin block, checkout UI, etc.)
const response = await fetch('shopify:admin/api/2026-01/graphql.json', {
  method: 'POST',
  body: JSON.stringify({
    query: `
      query getProduct($id: ID!) {
        product(id: $id) { title status }
      }
    `,
    variables: { id: productId }
  }),
});

const { data } = await response.json();
```

Enable in your app config:
```toml
[access.admin]
direct_api_mode = "online"
```

This is simpler than routing through your server and eliminates latency for read operations. Use it for simple CRUD in extensions.

---

## Common Patterns

### Search products by title

```graphql
query searchProducts($query: String!) {
  products(first: 20, query: $query) {
    edges {
      node { id title handle status }
    }
  }
}
# Variables: { "query": "title:*shirt*" }
```

### Get metafields for a resource

```graphql
query productMetafields($id: ID!) {
  product(id: $id) {
    metafields(first: 20) {
      edges {
        node {
          namespace
          key
          value
          type
        }
      }
    }
  }
}
```

### Set multiple metafields at once

```graphql
mutation setMetafields($metafields: [MetafieldsSetInput!]!) {
  metafieldsSet(metafields: $metafields) {
    metafields { id namespace key value }
    userErrors { field message }
  }
}
```

### Get app installation data

```graphql
query {
  currentAppInstallation {
    id
    metafields(first: 20) {
      edges {
        node { namespace key value }
      }
    }
  }
}
```
