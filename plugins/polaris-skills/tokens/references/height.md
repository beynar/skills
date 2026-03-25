# Height Tokens Reference

## Height Scale

### No Height
- `--p-height-0`: 0px - No height, hidden elements

### Extra Small Heights
- `--p-height-025`: 1px - Hairline height (dividers)
- `--p-height-050`: 2px - Minimal height
- `--p-height-100`: 4px - Very small height

### Small Heights
- `--p-height-150`: 6px - Small height
- `--p-height-200`: 8px - Small height (common for spacing)
- `--p-height-300`: 12px - Small-medium height

### Medium Heights
- `--p-height-400`: 16px - Medium height (buttons, form elements)
- `--p-height-500`: 20px - Medium height
- `--p-height-600`: 24px - Medium height (default row height)

### Medium-Large Heights
- `--p-height-700`: 28px - Medium-large height
- `--p-height-800`: 32px - Medium-large height

### Large Heights
- `--p-height-900`: 36px - Large height
- `--p-height-1000`: 40px - Large height (spacious components)

### Extra Large Heights
- `--p-height-1200`: 48px - Extra large height
- `--p-height-1600`: 64px - Very large height

### Huge Heights
- `--p-height-2000`: 80px - Huge height
- `--p-height-2400`: 96px - Very huge height
- `--p-height-2800`: 112px - Massive height
- `--p-height-3200`: 128px - Maximum height

## Height Token Equivalents with Spacing

Height tokens follow the same scale as spacing tokens. They can be used interchangeably:
- `--p-height-0` = `--p-space-0`
- `--p-height-025` = `--p-space-025`
- `--p-height-050` = `--p-space-050`
- `--p-height-100` = `--p-space-100`
- (and so on...)

## Common Component Heights

### Minimal Components
- **Divider line**: 1px (--p-height-025)
- **Hairline rule**: 1px (--p-height-025)

### Icon Heights
- **Small icon**: 16px (--p-height-400)
- **Medium icon**: 20px (--p-height-500)
- **Large icon**: 24px (--p-height-600)

### Control Elements
- **Button (compact)**: 32px (--p-height-800)
- **Button (standard)**: 36px (--p-height-900)
- **Button (large)**: 40px (--p-height-1000)

### Form Elements
- **Text input (compact)**: 28px (--p-height-700)
- **Text input (standard)**: 32px-36px (--p-height-800 or --p-height-900)
- **Text input (large)**: 40px (--p-height-1000)

### List Items & Rows
- **Compact list item**: 28px (--p-height-700)
- **Standard list item**: 32px-36px (--p-height-800 or --p-height-900)
- **Spacious list item**: 40px (--p-height-1000)
- **Table row**: 40px (--p-height-1000)

### Cards & Containers
- **Small card**: 60-96px (--p-height-1200 to --p-height-2400)
- **Medium card**: 96-128px (--p-height-2400 to --p-height-3200)
- **Large card**: 150px+

### Header/Navigation
- **Compact header**: 40px (--p-height-1000)
- **Standard header**: 48px (--p-height-1200)
- **Spacious header**: 56px+

### Avatar/Image
- **Small avatar**: 28px (--p-height-700)
- **Medium avatar**: 32px (--p-height-800)
- **Large avatar**: 40px (--p-height-1000)
- **Extra large avatar**: 48px (--p-height-1200)

## Usage Examples

### Text Input
```css
input {
  height: var(--p-height-900);
  padding: var(--p-space-200) var(--p-space-300);
  font-size: var(--p-font-size-400);
}
```

### Button
```css
button {
  height: var(--p-height-900);
  padding: var(--p-space-200) var(--p-space-400);
  font-size: var(--p-font-size-400);
}
```

### List Item
```css
.list-item {
  height: var(--p-height-900);
  padding: var(--p-space-200) var(--p-space-400);
}
```

### Table Row
```css
table row {
  height: var(--p-height-1000);
}
```

### Header
```css
.header {
  height: var(--p-height-1200);
  padding: var(--p-space-300) var(--p-space-400);
}
```

### Avatar
```css
.avatar {
  width: var(--p-height-900);
  height: var(--p-height-900);
  border-radius: var(--p-border-radius-full);
}
```

## Height vs. Line-Height vs. Spacing

- **Height**: Fixed height of an element
- **Line-height**: Vertical spacing within text (use font line-height tokens)
- **Spacing/Padding**: Internal space around content

### Combining with Line-Height
```css
.text-input {
  height: var(--p-height-900);
  line-height: var(--p-font-line-height-500);
  padding: var(--p-space-200) var(--p-space-300);
}
```

## Height with Flex/Grid

### Flex Usage
```css
.flex-container {
  display: flex;
  gap: var(--p-space-200);
}

.flex-item {
  height: var(--p-height-900);
}
```

### Grid Usage
```css
.grid {
  display: grid;
  grid-auto-rows: var(--p-height-1000);
  gap: var(--p-space-400);
}
```
