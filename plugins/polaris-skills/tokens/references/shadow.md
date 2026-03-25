# Shadow Tokens Reference

## Depth Shadows (Elevation)

### No Shadow
- `--p-shadow-0`: none
  - Use for: Flat elements with no elevation

### Shadow 100 (Subtle)
- `--p-shadow-100`: 0px 1px 0px 0px rgba(26, 26, 26, 0.07)
  - Use for: Subtle elevation, minimal depth
  - Elevation: Very subtle

### Shadow 200 (Light)
- `--p-shadow-200`: 0px 3px 1px -1px rgba(26, 26, 26, 0.07)
  - Use for: Light elevation
  - Elevation: Light depth

### Shadow 300 (Medium)
- `--p-shadow-300`: 0px 4px 6px -2px rgba(26, 26, 26, 0.20)
  - Use for: Cards, small lifted elements
  - Elevation: Medium depth

### Shadow 400 (Medium-Large)
- `--p-shadow-400`: 0px 8px 16px -4px rgba(26, 26, 26, 0.22)
  - Use for: Cards with more elevation, modals
  - Elevation: Medium-large depth

### Shadow 500 (Large)
- `--p-shadow-500`: 0px 12px 20px -8px rgba(26, 26, 26, 0.24)
  - Use for: Floating elements, prominent cards
  - Elevation: Large depth

### Shadow 600 (Extra Large)
- `--p-shadow-600`: 0px 20px 20px -8px rgba(26, 26, 26, 0.28)
  - Use for: Modals, overlay elements, maximum elevation
  - Elevation: Extra large depth

## Specialized Shadows

### Bevel Shadow
- `--p-shadow-bevel-100`: 1px 0px 0px 0px rgba(0, 0, 0, 0.13) inset, -1px 0px 0px 0px rgba(0, 0, 0, 0.13) inset, 0px -1px 0px 0px rgba(0, 0, 0, 0.17) inset, 0px 1px 0px 0px rgba(204, 204, 204, 0.5) inset
  - Use for: Embossed or beveled button effects, 3D appearance
  - Effect: Creates a three-dimensional beveled edge

### Inset Shadows

#### Inset 100 (Light Inset)
- `--p-shadow-inset-100`: 0px 1px 2px 0px rgba(26, 26, 26, 0.15) inset, 0px 1px 1px 0px rgba(26, 26, 26, 0.15) inset
  - Use for: Subtle inset effects, pressed button states
  - Effect: Light inset shadow

#### Inset 200 (Medium Inset)
- `--p-shadow-inset-200`: 0px 2px 1px 0px rgba(26, 26, 26, 0.20) inset, 1px 0px 1px 0px rgba(26, 26, 26, 0.12) inset, -1px 0px 1px 0px rgba(26, 26, 26, 0.12) inset
  - Use for: Stronger inset effects, depressed states
  - Effect: Medium inset shadow with side shadows

### Border Shadow (Inset Border Effect)
- `--p-shadow-border-inset`: 0px 0px 0px 1px rgba(0, 0, 0, 0.08) inset
  - Use for: Subtle inset border effect
  - Effect: Creates an inset border appearance without using border property

## Button-Specific Shadows

### Standard Button
- `--p-shadow-button`: 0px -1px 0px 0px #b5b5b5 inset, 0px 0px 0px 1px rgba(0, 0, 0, 0.1) inset, 0px 0.5px 0px 1.5px #FFF inset
  - Use for: Default button appearance
  - Effect: 3D embossed button effect

### Button Hover
- `--p-shadow-button-hover`: 0px 1px 0px 0px #EBEBEB inset, -1px 0px 0px 0px #EBEBEB inset, 1px 0px 0px 0px #EBEBEB inset, 0px -1px 0px 0px #CCC inset
  - Use for: Button hover state
  - Effect: Elevated/lifted button appearance

### Button Pressed (Inset)
- `--p-shadow-button-inset`: -1px 0px 1px 0px rgba(26, 26, 26, 0.122) inset, 1px 0px 1px 0px rgba(26, 26, 26, 0.122) inset, 0px 2px 1px 0px rgba(26, 26, 26, 0.2) inset
  - Use for: Button active/pressed state
  - Effect: Sunken/pressed button appearance

## Primary Button Shadows

### Primary Button
- `--p-shadow-button-primary`: 0px -1px 0px 1px rgba(0, 0, 0, 0.8) inset, 0px 0px 0px 1px rgba(48, 48, 48, 1) inset, 0px 0.5px 0px 1.5px rgba(255, 255, 255, 0.25) inset
  - Use for: Primary action buttons (default state)
  - Effect: Dark embossed appearance with subtle highlight

### Primary Button Hover
- `--p-shadow-button-primary-hover`: 0px 1px 0px 0px rgba(255, 255, 255, 0.24) inset, 1px 0px 0px 0px rgba(255, 255, 255, 0.20) inset, -1px 0px 0px 0px rgba(255, 255, 255, 0.20) inset, 0px -1px 0px 0px #000 inset, 0px -1px 0px 1px #1A1A1A
  - Use for: Primary button hover state
  - Effect: Brightened/lifted appearance

### Primary Button Pressed
- `--p-shadow-button-primary-inset`: 0px 3px 0px 0px rgb(0, 0, 0) inset
  - Use for: Primary button active/pressed state
  - Effect: Sunken appearance

## Critical Button Shadows

### Critical Button
- `--p-shadow-button-primary-critical`: 0px -1px 0px 1px rgba(142, 31, 11, 0.8) inset, 0px 0px 0px 1px rgba(181, 38, 11, 0.8) inset, 0px 0.5px 0px 1.5px rgba(255, 255, 255, 0.349) inset
  - Use for: Destructive/critical action buttons (default)
  - Effect: Dark red embossed appearance

### Critical Button Hover
- `--p-shadow-button-primary-critical-hover`: 0px 1px 0px 0px rgba(255, 255, 255, 0.48) inset, 1px 0px 0px 0px rgba(255, 255, 255, 0.20) inset, -1px 0px 0px 0px rgba(255, 255, 255, 0.20) inset, 0px -1.5px 0px 0px rgba(0, 0, 0, 0.25) inset
  - Use for: Critical button hover state
  - Effect: Brightened critical appearance

### Critical Button Pressed
- `--p-shadow-button-primary-critical-inset`: -1px 0px 1px 0px rgba(0, 0, 0, 0.2) inset, 1px 0px 1px 0px rgba(0, 0, 0, 0.2) inset, 0px 2px 0px 0px rgba(0, 0, 0, 0.6) inset
  - Use for: Critical button active/pressed state
  - Effect: Sunken critical appearance

## Success Button Shadows

### Success Button
- `--p-shadow-button-primary-success`: 0px -1px 0px 1px rgba(12, 81, 50, 0.8) inset, 0px 0px 0px 1px rgba(19, 111, 69, 0.8) inset, 0px 0.5px 0px 1.5px rgba(255, 255, 255, 0.251) inset
  - Use for: Success/positive action buttons (default)
  - Effect: Dark green embossed appearance

### Success Button Hover
- `--p-shadow-button-primary-success-hover`: 0px 1px 0px 0px rgba(255, 255, 255, 0.48) inset, 1px 0px 0px 0px rgba(255, 255, 255, 0.20) inset, -1px 0px 0px 0px rgba(255, 255, 255, 0.20) inset, 0px -1.5px 0px 0px rgba(0, 0, 0, 0.25) inset
  - Use for: Success button hover state
  - Effect: Brightened success appearance

### Success Button Pressed
- `--p-shadow-button-primary-success-inset`: -1px 0px 1px 0px rgba(0, 0, 0, 0.2) inset, 1px 0px 1px 0px rgba(0, 0, 0, 0.2) inset, 0px 2px 0px 0px rgba(0, 0, 0, 0.6) inset
  - Use for: Success button active/pressed state
  - Effect: Sunken success appearance

## Shadow Elevation Guidelines

### Component-to-Shadow Mapping
- **Flat elements**: --p-shadow-0
- **Tags, badges**: --p-shadow-100
- **Form inputs, list items**: --p-shadow-100 or --p-shadow-200
- **Cards, boxes**: --p-shadow-300 or --p-shadow-400
- **Floating panels, menus**: --p-shadow-400 or --p-shadow-500
- **Modals, overlays**: --p-shadow-500 or --p-shadow-600
- **Top-layer elements**: --p-shadow-600

### Button Shadow Strategy
- Use button-specific shadows for 3D embossed effects
- Default state: --p-shadow-button (or color-specific variants)
- Hover state: --p-shadow-button-hover (or color-specific variants)
- Active/pressed state: --p-shadow-button-inset (or color-specific variants)
