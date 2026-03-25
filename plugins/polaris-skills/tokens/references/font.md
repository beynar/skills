# Font Tokens Reference

## Font Families

### Sans Serif (Default)
- `--p-font-family-sans`: 'Inter', -apple-system, BlinkMacSystemFont, 'San Francisco', 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif
  - Primary typeface for all UI text

### Monospace
- `--p-font-family-mono`: ui-monospace, SFMono-Regular, 'SF Mono', Consolas, 'Liberation Mono', Menlo, monospace
  - For code blocks, terminal output, and technical text

## Font Sizes

Small Sizes:
- `--p-font-size-275`: 11px
- `--p-font-size-300`: 12px
- `--p-font-size-325`: 13px

Base Sizes:
- `--p-font-size-350`: 14px - Caption text
- `--p-font-size-400`: 16px - Default body text
- `--p-font-size-450`: 18px

Medium Sizes:
- `--p-font-size-500`: 20px
- `--p-font-size-550`: 22px

Large Sizes:
- `--p-font-size-600`: 24px
- `--p-font-size-750`: 30px

Display Sizes:
- `--p-font-size-800`: 32px
- `--p-font-size-900`: 36px
- `--p-font-size-1000`: 40px - Large headings/display

## Font Weights

- `--p-font-weight-regular`: 450 - Default weight, body text
- `--p-font-weight-medium`: 550 - Slightly emphasized text
- `--p-font-weight-semibold`: 650 - Labels, subheadings
- `--p-font-weight-bold`: 700 - Headings, strong emphasis

## Letter Spacing

Negative (Tighter):
- `--p-font-letter-spacing-densest`: -0.54px - Tightest spacing
- `--p-font-letter-spacing-denser`: -0.3px - Very tight spacing
- `--p-font-letter-spacing-dense`: -0.2px - Tight spacing

Normal:
- `--p-font-letter-spacing-normal`: 0px - Default spacing

## Line Heights

Compact Heights:
- `--p-font-line-height-300`: 12px - Minimal line height
- `--p-font-line-height-400`: 16px - Compact spacing

Standard Heights:
- `--p-font-line-height-500`: 20px - Default line height (1.25 ratio)
- `--p-font-line-height-600`: 24px - Comfortable reading

Comfortable Heights:
- `--p-font-line-height-700`: 28px
- `--p-font-line-height-800`: 32px

Spacious Heights:
- `--p-font-line-height-1000`: 40px
- `--p-font-line-height-1200`: 48px - Spacious for body text

## Usage Guidelines

### Font Size Combinations
- **Display**: --p-font-size-900 or --p-font-size-1000 with --p-font-weight-bold
- **Heading**: --p-font-size-600 with --p-font-weight-semibold or --p-font-weight-bold
- **Subheading**: --p-font-size-450 with --p-font-weight-semibold
- **Body**: --p-font-size-400 with --p-font-weight-regular and --p-font-line-height-500
- **Caption**: --p-font-size-350 with --p-font-weight-regular and --p-font-line-height-400
- **Small Text**: --p-font-size-300 with --p-font-weight-regular
