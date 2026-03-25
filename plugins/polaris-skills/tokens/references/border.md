# Border Tokens Reference

## Border Radius

### No Radius
- `--p-border-radius-0`: 0px - Sharp corners (no rounding)

### Extra Small Radius
- `--p-border-radius-050`: 2px - Minimal rounding
- `--p-border-radius-100`: 4px - Small rounding

### Small Radius
- `--p-border-radius-150`: 6px - Small-medium rounding
- `--p-border-radius-200`: 8px - Standard rounding

### Medium Radius
- `--p-border-radius-300`: 12px - Medium rounding
- `--p-border-radius-400`: 16px - Medium-large rounding

### Large Radius
- `--p-border-radius-500`: 20px - Large rounding
- `--p-border-radius-750`: 30px - Extra large rounding

### Full Radius
- `--p-border-radius-full`: 9999px - Fully rounded (perfect circles/pills)

## Border Width

### No Border
- `--p-border-width-0`: 0px - No border

### Hairline Border
- `--p-border-width-0165`: 0.66px - Hairline border (subtle)

### Thin Border
- `--p-border-width-025`: 1px - Standard thin border (most common)

### Medium Border
- `--p-border-width-050`: 2px - Medium border

### Thick Border
- `--p-border-width-100`: 4px - Thick border (for emphasis or focus states)

## Usage Guidelines

### Border Radius by Component Type

#### Controls (Buttons, Inputs)
- Use `--p-border-radius-100` (4px) or `--p-border-radius-200` (8px)
- For pill-shaped buttons: `--p-border-radius-full`

#### Cards & Surfaces
- Use `--p-border-radius-200` (8px) or `--p-border-radius-300` (12px)
- Larger cards may use `--p-border-radius-400` (16px)

#### Popovers & Modals
- Use `--p-border-radius-300` (12px) or `--p-border-radius-400` (16px)

#### Badges & Small Elements
- Use `--p-border-radius-100` (4px) or `--p-border-radius-200` (8px)
- For circular badges: `--p-border-radius-full`

#### Avatars
- Circular avatars: `--p-border-radius-full`
- Square avatars: `--p-border-radius-0` or `--p-border-radius-100`

### Border Width by Context

#### Standard Borders
- Use `--p-border-width-025` (1px) for:
  - Card borders
  - Input field borders
  - Table cell borders
  - Subtle element separation

#### Focus/Active Borders
- Use `--p-border-width-025` (1px) with focus color
- Or `--p-border-width-050` (2px) for stronger emphasis

#### Emphasis Borders
- Use `--p-border-width-100` (4px) for:
  - Alert banners
  - Highlight states
  - Drag-and-drop targets

#### Hairline Dividers
- Use `--p-border-width-0165` (0.66px) for:
  - Section separators
  - Very subtle dividing lines

## Common Border Combinations

### Default Card Border
```css
border-width: var(--p-border-width-025);
border-radius: var(--p-border-radius-200);
border-color: var(--p-color-border);
```

### Input Field Border
```css
border-width: var(--p-border-width-025);
border-radius: var(--p-border-radius-100);
border-color: var(--p-color-border);
```

### Focus State Border
```css
border-width: var(--p-border-width-025);
border-color: var(--p-color-border-emphasis);
```

### Pill Button
```css
border-width: var(--p-border-width-025);
border-radius: var(--p-border-radius-full);
```

### Critical/Alert Border
```css
border-width: var(--p-border-width-050);
border-radius: var(--p-border-radius-200);
border-color: var(--p-color-border-critical);
```

### Rounded Button
```css
border-width: var(--p-border-width-025);
border-radius: var(--p-border-radius-200);
```
