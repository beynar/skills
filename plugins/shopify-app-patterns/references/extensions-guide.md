# Shopify Extensions Guide

## Table of Contents
1. [Extension Fundamentals](#extension-fundamentals)
2. [Admin Action Extensions](#admin-actions)
3. [Admin Block Extensions](#admin-blocks)
4. [Checkout UI Extensions](#checkout-ui)
5. [Theme App Extensions](#theme-app-extensions)
6. [POS UI Extensions](#pos-ui-extensions)
7. [Extension Configuration](#extension-configuration)
8. [Deployment & Versioning](#deployment)

---

## Extension Fundamentals

Extensions are self-contained units of functionality that plug into Shopify surfaces. They are:
- **Hosted by Shopify** — no infrastructure to manage
- **Sandboxed** — limited APIs, strict size limits (64 KB compressed for UI extensions)
- **Versioned together** — all extensions deploy as a single app version
- **Declaratively configured** — via `shopify.extension.toml`

### Generating an extension

```bash
shopify app generate extension
# Interactive prompts let you choose extension type
```

This creates a directory under `extensions/` with:
```
extensions/my-extension/
├── shopify.extension.toml   # Configuration
├── src/
│   └── index.jsx            # Entry point (for UI extensions)
└── package.json
```

### Extension lifecycle

1. **Generate** → `shopify app generate extension`
2. **Develop** → `shopify app dev` (live preview with hot reload)
3. **Deploy** → `shopify app deploy` (creates a new app version)
4. **Review** (some types) → Shopify reviews before release
5. **Release** → Active version serves all installed stores

---

## Admin Action Extensions

Admin actions appear as menu items on resource pages (Products, Orders, Customers) and open as modals. They're ideal for transactional workflows: "Create issue", "Send notification", "Apply tag".

### Configuration

```toml
# shopify.extension.toml
api_version = "2026-01"

[[extensions]]
name = "Create Issue"
handle = "create-issue"
type = "ui_extension"

[[extensions.targeting]]
module = "./src/ActionExtension.jsx"
target = "admin.product-details.action.render"
```

### Target options for actions

| Target | Where it appears |
|--------|-----------------|
| `admin.product-details.action.render` | Product detail page |
| `admin.order-details.action.render` | Order detail page |
| `admin.customer-details.action.render` | Customer detail page |
| `admin.product-index.selection-action.render` | Bulk action on product list |
| `admin.order-index.selection-action.render` | Bulk action on order list |

### Implementation pattern

```jsx
import { useEffect, useState } from "react";
import {
  reactExtension,
  useApi,
  AdminAction,
  BlockStack,
  TextField,
  Button,
  Text,
} from "@shopify/ui-extensions-react/admin";

const TARGET = "admin.product-details.action.render";

export default reactExtension(TARGET, () => <CreateIssueAction />);

function CreateIssueAction() {
  const { close, data } = useApi(TARGET);
  const [title, setTitle] = useState("");
  const [description, setDescription] = useState("");
  const productId = data.selected?.[0]?.id;

  async function handleSubmit() {
    // Call Admin GraphQL directly from extension
    const response = await fetch("shopify:admin/api/2026-01/graphql.json", {
      method: "POST",
      body: JSON.stringify({
        query: `mutation setMetafields($metafields: [MetafieldsSetInput!]!) {
          metafieldsSet(metafields: $metafields) {
            metafields { id }
            userErrors { field message }
          }
        }`,
        variables: {
          metafields: [{
            ownerId: productId,
            namespace: "$app",
            key: "issue",
            type: "json",
            value: JSON.stringify({ title, description, status: "open" }),
          }],
        },
      }),
    });

    close();
  }

  return (
    <AdminAction
      title="Create Issue"
      primaryAction={<Button onPress={handleSubmit}>Create</Button>}
      secondaryAction={<Button onPress={close}>Cancel</Button>}
    >
      <BlockStack gap="base">
        <TextField label="Issue title" value={title} onChange={setTitle} />
        <TextField
          label="Description"
          value={description}
          onChange={setDescription}
          multiline={3}
        />
      </BlockStack>
    </AdminAction>
  );
}
```

---

## Admin Block Extensions

Admin blocks are persistent cards on resource detail pages. Unlike actions (which are transient modals), blocks are always visible. Use them to display contextual information or inline editing forms.

### Configuration

```toml
[[extensions]]
name = "Product Issues"
handle = "product-issues"
type = "ui_extension"

[[extensions.targeting]]
module = "./src/BlockExtension.jsx"
target = "admin.product-details.block.render"
```

### Implementation pattern

```jsx
import { useEffect, useState } from "react";
import {
  reactExtension,
  useApi,
  AdminBlock,
  BlockStack,
  InlineStack,
  Text,
  Badge,
  Divider,
} from "@shopify/ui-extensions-react/admin";

const TARGET = "admin.product-details.block.render";

export default reactExtension(TARGET, () => <IssuesBlock />);

function IssuesBlock() {
  const { data } = useApi(TARGET);
  const [issues, setIssues] = useState([]);

  useEffect(() => {
    async function loadIssues() {
      const productId = data.selected?.[0]?.id;
      const response = await fetch("shopify:admin/api/2026-01/graphql.json", {
        method: "POST",
        body: JSON.stringify({
          query: `query ($id: ID!) {
            product(id: $id) {
              metafield(namespace: "$app", key: "issues") { value }
            }
          }`,
          variables: { id: productId },
        }),
      });
      const json = await response.json();
      const raw = json.data?.product?.metafield?.value;
      if (raw) setIssues(JSON.parse(raw));
    }
    loadIssues();
  }, []);

  return (
    <AdminBlock title="Product Issues">
      <BlockStack gap="base">
        {issues.length === 0 ? (
          <Text>No issues reported.</Text>
        ) : (
          issues.map((issue, i) => (
            <BlockStack key={i} gap="extraTight">
              <InlineStack gap="base" blockAlignment="center">
                <Text fontWeight="bold">{issue.title}</Text>
                <Badge tone={issue.status === "open" ? "warning" : "success"}>
                  {issue.status}
                </Badge>
              </InlineStack>
              <Text>{issue.description}</Text>
              {i < issues.length - 1 && <Divider />}
            </BlockStack>
          ))
        )}
      </BlockStack>
    </AdminBlock>
  );
}
```

---

## Checkout UI Extensions

Checkout UI extensions add custom UI elements to the checkout flow. They use a separate component library (`@shopify/ui-extensions-react/checkout`).

### Common targets

| Target | Location |
|--------|----------|
| `purchase.checkout.block.render` | Configurable block (merchant places it) |
| `purchase.checkout.cart-line-item.render-after` | After each cart line item |
| `purchase.checkout.shipping-option-list.render-after` | After shipping options |
| `purchase.thank-you.block.render` | Thank you page |

### Configuration

```toml
[[extensions]]
name = "Upsell Banner"
handle = "upsell-banner"
type = "ui_extension"

[[extensions.targeting]]
module = "./src/Checkout.jsx"
target = "purchase.checkout.block.render"
```

### Implementation pattern

```jsx
import {
  reactExtension,
  Banner,
  useCartLines,
  useApplyCartLinesChange,
} from "@shopify/ui-extensions-react/checkout";

export default reactExtension(
  "purchase.checkout.block.render",
  () => <UpsellBanner />
);

function UpsellBanner() {
  const cartLines = useCartLines();
  const applyCartLinesChange = useApplyCartLinesChange();

  const hasWidget = cartLines.some(
    line => line.merchandise.product.handle === "premium-widget"
  );

  if (hasWidget) return null;

  return (
    <Banner title="Add a Premium Widget?" status="info">
      Complete your order with our best-selling accessory — 15% off when
      added now.
    </Banner>
  );
}
```

### Multi-page checkout extensions

A single extension can render on multiple checkout pages:

```toml
[[extensions.targeting]]
module = "./src/Checkout.jsx"
target = "purchase.checkout.block.render"

[[extensions.targeting]]
module = "./src/ThankYou.jsx"
target = "purchase.thank-you.block.render"
```

### Network requests from checkout extensions

Checkout extensions run in a sandboxed environment. For requests to your own server:
- Your server must return proper CORS headers
- Use `fetch()` with the extension's built-in capabilities
- Session tokens are available via the checkout extension API

---

## Theme App Extensions

Theme app extensions integrate your app with online store themes without editing theme code. Mandatory for App Store apps.

### App blocks

App blocks render inline on pages. Merchants place them via the theme editor.

```
extensions/theme-extension/
├── shopify.extension.toml
├── blocks/
│   └── product-badge.liquid
├── assets/
│   └── badge-styles.css
└── locales/
    └── en.default.json
```

**Configuration:**
```toml
api_version = "2026-01"
type = "theme_app_extension"

[[extensions]]
name = "Product Badge"
handle = "product-badge"
```

**Liquid block (blocks/product-badge.liquid):**
```liquid
{% schema %}
{
  "name": "Product Badge",
  "target": "section",
  "settings": [
    {
      "type": "text",
      "id": "badge_text",
      "label": "Badge Text",
      "default": "New Arrival"
    },
    {
      "type": "color",
      "id": "badge_color",
      "label": "Badge Color",
      "default": "#FF0000"
    }
  ]
}
{% endschema %}

<div class="app-badge" style="background-color: {{ block.settings.badge_color }}">
  {{ block.settings.badge_text }}
</div>

{% stylesheet %}
  .app-badge {
    display: inline-block;
    padding: 4px 12px;
    border-radius: 4px;
    color: white;
    font-size: 12px;
    font-weight: bold;
  }
{% endstylesheet %}
```

### App embed blocks

Embed blocks are for floating/overlay elements — chat widgets, notification bars, analytics scripts. They don't have a visible insertion point in the theme editor.

```liquid
{% schema %}
{
  "name": "Chat Widget",
  "target": "body",
  "settings": [
    {
      "type": "text",
      "id": "api_key",
      "label": "Chat API Key"
    }
  ]
}
{% endschema %}

<script src="{{ 'chat-widget.js' | asset_url }}" defer></script>
<script>
  document.addEventListener('DOMContentLoaded', function() {
    initChat({ apiKey: {{ block.settings.api_key | json }} });
  });
</script>
```

### Verifying theme support

Not all themes support app blocks. Check programmatically:

```graphql
query {
  theme(id: "gid://shopify/Theme/123") {
    name
    role
    files(first: 10, filenames: ["templates/product.json"]) {
      nodes {
        filename
        body {
          ... on OnlineStoreThemeFileBodyText { content }
        }
      }
    }
  }
}
```

If the template file is `.json` (not `.liquid`), it supports Online Store 2.0 sections and app blocks.

---

## POS UI Extensions

POS extensions render as native components on iOS/Android for Shopify's point-of-sale app.

### Target types

| Type | Target example | What it renders |
|------|---------------|-----------------|
| Tile | `pos.home.tile.render` | Smart grid tile on home screen |
| Action (menu) | `pos.home.action.menu-item.render` | Button in a menu |
| Action (modal) | `pos.home.action.render` | Modal when button is tapped |
| Block | `pos.product-details.block.render` | Section on product detail screen |

### Configuration

```toml
api_version = "2026-01"

[[extensions]]
name = "Loyalty Check"
handle = "loyalty-check"
type = "pos_ui_extension"

[[extensions.targeting]]
module = "./src/Tile.jsx"
target = "pos.home.tile.render"

[[extensions.targeting]]
module = "./src/Modal.jsx"
target = "pos.home.action.render"
```

### POS extension pattern

```jsx
import { Tile, Text } from "@shopify/ui-extensions-react/point-of-sale";
import { reactExtension } from "@shopify/ui-extensions-react/point-of-sale";

export default reactExtension("pos.home.tile.render", () => <LoyaltyTile />);

function LoyaltyTile() {
  return (
    <Tile title="Loyalty" subtitle="Check points" onPress={() => {
      // Opens the action modal
    }}>
      <Text>Tap to check customer loyalty points</Text>
    </Tile>
  );
}
```

POS extensions use the `cart`, `navigation`, `toast`, and other target APIs. These APIs are reactive — subscribe to changes and your UI updates automatically.

---

## Extension Configuration

All extensions use `shopify.extension.toml` for configuration. Common patterns:

### Access scopes for extensions

```toml
[extensions.capabilities]
api_access = true          # Can make Admin API calls
network_access = true      # Can make external HTTP requests
block_progress = true      # Can block checkout progression (checkout only)
```

### Extension points (targeting)

```toml
# Single target
[[extensions.targeting]]
module = "./src/Extension.jsx"
target = "admin.product-details.action.render"

# Multiple targets from same extension
[[extensions.targeting]]
module = "./src/ProductAction.jsx"
target = "admin.product-details.action.render"

[[extensions.targeting]]
module = "./src/OrderAction.jsx"
target = "admin.order-details.action.render"
```

### Settings (merchant-configurable)

For checkout and theme extensions, define settings that merchants can configure:

```toml
[extensions.settings]
[[extensions.settings.fields]]
key = "banner_text"
type = "single_line_text_field"
name = "Banner Text"

[[extensions.settings.fields]]
key = "show_icon"
type = "boolean"
name = "Show Icon"
```

---

## Deployment

### Bundle size management

UI extensions have a strict **64 KB compressed** size limit. To stay under:

1. Import only what you use from component libraries
2. Avoid large dependencies — most utilities can be written inline
3. Use the esbuild metafile (`.metafile.json` in `dist/`) to analyze what's contributing to size
4. Upload the metafile to [esbuild.github.io/analyze](https://esbuild.github.io/analyze/) for visualization

### Version management

```bash
# Preview locally
shopify app dev

# Deploy new version (creates + releases)
shopify app deploy

# Rollback via Dev Dashboard if needed
```

All extensions ship together as one app version. You can't release extensions individually. If one extension needs review, the entire version waits for approval.

### Removing an extension

Delete the extension directory from `extensions/`, then redeploy. The extension is permanently removed from all installed stores.
