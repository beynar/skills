# Space Tokens Reference

## Spacing Scale

### Base Scale (Pixels)
- `--p-space-0`: 0px - No spacing
- `--p-space-025`: 1px - Hairline spacing
- `--p-space-050`: 2px - Minimal spacing
- `--p-space-100`: 4px - Extra small spacing
- `--p-space-150`: 6px - Small spacing
- `--p-space-200`: 8px - Small spacing (common for padding)
- `--p-space-300`: 12px - Small-medium spacing
- `--p-space-400`: 16px - Medium spacing (common for padding and margins)
- `--p-space-500`: 20px - Medium spacing
- `--p-space-600`: 24px - Medium-large spacing
- `--p-space-800`: 32px - Large spacing
- `--p-space-1000`: 40px - Large spacing
- `--p-space-1200`: 48px - Extra large spacing
- `--p-space-1600`: 64px - Very large spacing
- `--p-space-2000`: 80px - Extra large spacing
- `--p-space-2400`: 96px - Very extra large spacing
- `--p-space-2800`: 112px - Huge spacing
- `--p-space-3200`: 128px - Maximum spacing

## Component-Specific Spacing

### Button Groups
- `--p-space-button-group-gap`: var(--p-space-200) - Gap between buttons in button groups (8px)

### Cards
- `--p-space-card-gap`: var(--p-space-400) - Gap between card sections (16px)
- `--p-space-card-padding`: var(--p-space-400) - Internal padding for cards (16px)

### Tables
- `--p-space-table-cell-padding`: var(--p-space-150) - Internal padding for table cells (6px)

## Common Usage Patterns

### Micro Spacing
- Use `--p-space-025`, `--p-space-050`, `--p-space-100` for:
  - Tight element grouping
  - Icon-to-text spacing
  - Subtle visual separation

### Small Spacing
- Use `--p-space-150`, `--p-space-200`, `--p-space-300` for:
  - Internal element padding
  - Small component gaps
  - List item spacing

### Medium Spacing
- Use `--p-space-400`, `--p-space-500`, `--p-space-600` for:
  - Card padding
  - Section margins
  - Button groups
  - Form field spacing

### Large Spacing
- Use `--p-space-800`, `--p-space-1000`, `--p-space-1200` for:
  - Page sections
  - Major layout divisions
  - Top/bottom margins

### Extra Large Spacing
- Use `--p-space-1600`, `--p-space-2000`, `--p-space-2400`, `--p-space-2800`, `--p-space-3200` for:
  - Full-page layout spacing
  - Major section separation
  - Container widths/heights
