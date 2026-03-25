# Z-Index Tokens Reference

## Z-Index Scale

### Auto (Default)
- `--p-z-index-0`: auto
  - Use for: Most elements with no stacking context needed
  - Behavior: Uses natural document flow

### Base Level
- `--p-z-index-1`: 100
  - Use for: Slightly elevated elements
  - Above: Default flow

### Dropdown/Popover Level
- `--p-z-index-2`: 400
  - Use for: Dropdown menus, tooltips, popovers
  - Above: Base elements
  - Below: Modals

### Modal/Overlay Level
- `--p-z-index-3`: 510
  - Use for: Modal dialogs, overlay backgrounds
  - Above: Dropdowns
  - Primary modal level

### Modal Content & Floating Elements
- `--p-z-index-4`: 512
  - Use for: Content within modals, floating panels
  - Above: Modal backgrounds
  - Secondary modal level

### Sticky/Fixed Elements
- `--p-z-index-5`: 513
  - Use for: Sticky headers, fixed navigation
  - Above: Modal content
  - Good for sticky positioning

### Header/Navigation Level
- `--p-z-index-6`: 514
  - Use for: Fixed headers, main navigation bars
  - Above: Sticky elements

### Secondary Sticky/Fixed
- `--p-z-index-7`: 515
  - Use for: Secondary fixed elements
  - Above: Main navigation

### Tertiary Sticky/Fixed
- `--p-z-index-8`: 516
  - Use for: Tertiary fixed elements

### Notification/Toast Level
- `--p-z-index-9`: 517
  - Use for: Toast messages, notifications
  - Above: Most page content

### Alert/Banner Level
- `--p-z-index-10`: 518
  - Use for: Alert banners, critical messages

### Popover Above Everything
- `--p-z-index-11`: 519
  - Use for: Context menus, critical popovers

### Topmost Level
- `--p-z-index-12`: 520
  - Use for: Highest-level elements
  - Maximum recommended z-index

## Z-Index Stacking Context

### Stacking Order (Low to High)
1. **Auto (z-index: auto)** - Default elements
2. **z-index-1 (100)** - Slightly elevated content
3. **z-index-2 (400)** - Dropdowns, tooltips, popovers
4. **z-index-3 (510)** - Modal overlay backgrounds
5. **z-index-4 (512)** - Modal dialog content
6. **z-index-5 (513)** - Sticky/fixed positioning
7. **z-index-6 (514)** - Fixed headers/navigation
8. **z-index-7-8 (515-516)** - Additional fixed elements
9. **z-index-9 (517)** - Toast notifications
10. **z-index-10 (518)** - Alert banners
11. **z-index-11 (519)** - Critical popovers
12. **z-index-12 (520)** - Topmost elements

## Common Component Z-Index Mapping

### Standard Components
- **Buttons**: z-index-0 (auto)
- **Cards**: z-index-0 (auto)
- **Form inputs**: z-index-0 (auto)
- **List items**: z-index-0 (auto)

### Dropdowns & Menus
- **Dropdown list**: z-index-2 (400)
- **Autocomplete popup**: z-index-2 (400)
- **Select menu**: z-index-2 (400)
- **Context menu**: z-index-11 (519)

### Floating Elements
- **Tooltip**: z-index-2 (400)
- **Popover**: z-index-2 (400)
- **Floating action button**: z-index-5 (513)

### Modals & Overlays
- **Modal overlay/backdrop**: z-index-3 (510)
- **Modal dialog**: z-index-4 (512)
- **Modal content (buttons, inputs)**: z-index-4 (512)

### Navigation & Headers
- **Sticky header**: z-index-5 or z-index-6 (513-514)
- **Fixed navigation**: z-index-6 (514)
- **Fixed sidebar**: z-index-6 (514)
- **Top bar**: z-index-6 (514)

### Notifications & Alerts
- **Toast notification**: z-index-9 (517)
- **Banner**: z-index-10 (518)
- **Alert popup**: z-index-10 (518)
- **Critical alert**: z-index-11 (519)

## Z-Index Best Practices

### Always Use Tokens
- Never use arbitrary z-index values
- Always reference a z-index token from this list
- Maintains consistency across the application

### Modal & Overlay Guidelines
- Modal overlay: z-index-3
- Modal content: z-index-4
- Modals should not contain sticky/fixed elements, or if they do, use z-index-3 or z-index-4

### Navigation Guidelines
- Fixed headers: z-index-6
- Sticky headers: z-index-5
- Avoid z-index higher than z-index-7 for navigation

### Notification Guidelines
- Toast notifications: z-index-9
- Alert banners: z-index-10
- Critical alerts: z-index-11

### Dropdown Guidelines
- Dropdowns should use z-index-2 (appears above base content, below modals)
- Exception: Context menus appearing above modals should use z-index-11

### Spacing Between Values
- There are gaps in the z-index sequence (100, 400, 510-520)
- These gaps allow flexibility for component-specific stacking
- Do not create custom z-index values between tokens

## Example CSS Usage

### Fixed Header
```css
.header {
  position: fixed;
  top: 0;
  z-index: var(--p-z-index-6);
}
```

### Dropdown Menu
```css
.dropdown {
  position: absolute;
  z-index: var(--p-z-index-2);
}
```

### Modal Dialog
```css
.modal-overlay {
  position: fixed;
  z-index: var(--p-z-index-3);
}

.modal {
  position: relative;
  z-index: var(--p-z-index-4);
}
```

### Toast Notification
```css
.toast {
  position: fixed;
  z-index: var(--p-z-index-9);
}
```

### Sticky Element
```css
.sticky-element {
  position: sticky;
  z-index: var(--p-z-index-5);
}
```
