# Width Tokens Reference

## Width Scale

### No Width
- `--p-width-0`: 0px - No width, hidden elements

### Extra Small Widths
- `--p-width-025`: 1px - Hairline width (vertical dividers)
- `--p-width-050`: 2px - Minimal width
- `--p-width-100`: 4px - Very small width

### Small Widths
- `--p-width-150`: 6px - Small width
- `--p-width-200`: 8px - Small width (spacing unit)
- `--p-width-300`: 12px - Small-medium width

### Medium Widths
- `--p-width-400`: 16px - Medium width (sidebar, narrow column)
- `--p-width-500`: 20px - Medium width
- `--p-width-600`: 24px - Medium width

### Medium-Large Widths
- `--p-width-700`: 28px - Medium-large width
- `--p-width-800`: 32px - Medium-large width

### Large Widths
- `--p-width-900`: 36px - Large width
- `--p-width-1000`: 40px - Large width

### Extra Large Widths
- `--p-width-1200`: 48px - Extra large width
- `--p-width-1600`: 64px - Very large width

### Huge Widths
- `--p-width-2000`: 80px - Huge width
- `--p-width-2400`: 96px - Very huge width
- `--p-width-2800`: 112px - Massive width
- `--p-width-3200`: 128px - Maximum width

## Width Token Equivalents with Spacing

Width tokens follow the same scale as spacing tokens. They can be used interchangeably:
- `--p-width-0` = `--p-space-0`
- `--p-width-025` = `--p-space-025`
- `--p-width-050` = `--p-space-050`
- `--p-width-100` = `--p-space-100`
- (and so on...)

## Common Component Widths

### Minimal Components
- **Divider line**: 1px (--p-width-025)
- **Vertical separator**: 1px (--p-width-025)
- **Thin border**: 2px (--p-width-050)

### Icon Sizes
- **Small icon**: 16px (--p-width-400)
- **Medium icon**: 20px (--p-width-500)
- **Large icon**: 24px (--p-width-600)

### Avatar Sizes
- **Small avatar**: 28px (--p-width-700)
- **Medium avatar**: 32px (--p-width-800)
- **Large avatar**: 40px (--p-width-1000)
- **Extra large avatar**: 48px (--p-width-1200)

### Navigation Elements
- **Icon button**: 40px (--p-width-1000)
- **Small sidebar**: 64px (--p-width-1600)
- **Narrow sidebar**: 80px (--p-width-2000)
- **Standard sidebar**: 96-128px (--p-width-2400 to --p-width-3200)

### Form Elements
- **Checkbox/Radio**: 16px (--p-width-400)
- **Toggle switch**: 40px (--p-width-1000)
- **Input field**: 100% or container width

### Cards & Containers
- **Narrow card**: 96-128px (--p-width-2400 to --p-width-3200)
- **Medium card**: 150-300px
- **Wide card**: 300px+

### Modal Dialog
- **Small modal**: 320px
- **Standard modal**: 480px
- **Large modal**: 640px+

### Dropdown & Menu
- **Min dropdown width**: 150px
- **Max dropdown width**: 400px

## Sidebar & Navigation Widths

### Compact Sidebar
- `--p-width-1600`: 64px - Icon-only sidebar

### Narrow Sidebar
- `--p-width-2000`: 80px - Narrow sidebar with icons and labels

### Standard Sidebar
- `--p-width-2400`: 96px - Small standard sidebar
- `--p-width-3200`: 128px - Standard sidebar

### Wide Sidebar
- 200px+ for expanded navigation

## Max-Width for Content

### Container Max-Widths
```css
.content-container {
  max-width: 1200px; /* or var(--p-breakpoints-lg) */
}

.narrow-content {
  max-width: 600px;
}

.wide-content {
  max-width: 1400px; /* or var(--p-breakpoints-xl) */
}
```

## Usage Examples

### Sidebar
```css
.sidebar {
  width: var(--p-width-3200);
  background: var(--p-color-bg-surface);
}
```

### Avatar
```css
.avatar {
  width: var(--p-width-900);
  height: var(--p-width-900);
  border-radius: var(--p-border-radius-full);
}
```

### Icon Button
```css
.icon-button {
  width: var(--p-width-1000);
  height: var(--p-width-1000);
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### Checkbox
```css
input[type="checkbox"] {
  width: var(--p-width-400);
  height: var(--p-width-400);
}
```

### Toggle Switch
```css
.toggle {
  width: var(--p-width-1000);
  height: calc(var(--p-width-1000) / 2);
}
```

### Vertical Divider
```css
.divider {
  width: var(--p-width-025);
  background: var(--p-color-border);
}
```

## Width with Responsive Design

### Mobile-First Width Approach
```css
.column {
  width: 100%; /* Mobile */
}

@media (min-width: var(--p-breakpoints-sm)) {
  .column {
    width: 50%;
  }
}

@media (min-width: var(--p-breakpoints-md)) {
  .column {
    width: 33.333%;
  }
}

@media (min-width: var(--p-breakpoints-lg)) {
  .column {
    width: 25%;
  }
}
```

## Width with Flex Layout

### Flex Width Usage
```css
.flex-container {
  display: flex;
  gap: var(--p-space-400);
}

.flex-item-auto {
  flex: 1;
}

.flex-item-fixed {
  width: var(--p-width-3200);
  flex-shrink: 0;
}
```

## Width with Grid Layout

### Grid Column Widths
```css
.grid {
  display: grid;
  grid-template-columns: var(--p-width-3200) 1fr;
  gap: var(--p-space-400);
}

.sidebar {
  width: var(--p-width-3200);
}

.main {
  width: 100%;
}
```

## Min-Width Guidelines

### Min-Width Values
```css
.modal {
  min-width: 320px;
  max-width: 90vw;
}

.card {
  min-width: 150px;
}

.button {
  min-width: var(--p-width-1200);
}
```
