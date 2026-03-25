# Breakpoints Tokens Reference

## Breakpoint Values

### Extra Small (XS)
- `--p-breakpoints-xs`: 0px
  - Mobile-first starting point
  - Use for: Base/default styles (mobile-first approach recommended)

### Small (SM)
- `--p-breakpoints-sm`: 490px
  - Small mobile devices and larger
  - Use for: Tablets in portrait mode
  - Typical device width: ~490-600px

### Medium (MD)
- `--p-breakpoints-md`: 768px
  - Tablet devices
  - Use for: Tablet landscape and small desktops
  - Typical device width: ~768-1040px

### Large (LG)
- `--p-breakpoints-lg`: 1040px
  - Desktop computers
  - Use for: Full desktop layouts
  - Typical device width: ~1040-1440px

### Extra Large (XL)
- `--p-breakpoints-xl`: 1440px
  - Large desktop screens
  - Use for: Large monitors and wide displays
  - Typical device width: 1440px and above

## Media Query Usage in CSS

### CSS Media Queries
```css
/* Mobile-first approach (recommended) */
@media (min-width: var(--p-breakpoints-sm)) {
  /* Styles for SM and above */
}

@media (min-width: var(--p-breakpoints-md)) {
  /* Styles for MD and above */
}

@media (min-width: var(--p-breakpoints-lg)) {
  /* Styles for LG and above */
}

@media (min-width: var(--p-breakpoints-xl)) {
  /* Styles for XL and above */
}

/* Downward queries (less common) */
@media (max-width: 489px) {
  /* Styles for XS only */
}
```

## Sass Variables for Media Queries

Import the media-queries file from Polaris tokens:
```scss
@import 'path/to/node_modules/@shopify/polaris-tokens/dist/scss/media-queries';
```

### Generated Sass Variables

#### XS (Extra Small) - Mobile
- `$p-breakpoints-xs-up`: (min-width: 0em) - 0px and above
- `$p-breakpoints-xs-down`: (max-width: -0.0025em) - Below 0px (matches nothing)
- `$p-breakpoints-xs-only`: (min-width: 0em) and (max-width: 30.6225em) - 0-490px

#### SM (Small) - Mobile/Tablet Portrait
- `$p-breakpoints-sm-up`: (min-width: 30.625em) - 490px and above
- `$p-breakpoints-sm-down`: (max-width: 30.6225em) - Below 490px
- `$p-breakpoints-sm-only`: (min-width: 30.625em) and (max-width: 47.9975em) - 490-768px

#### MD (Medium) - Tablet Landscape
- `$p-breakpoints-md-up`: (min-width: 48em) - 768px and above
- `$p-breakpoints-md-down`: (max-width: 47.9975em) - Below 768px
- `$p-breakpoints-md-only`: (min-width: 48em) and (max-width: 64.9975em) - 768-1040px

#### LG (Large) - Desktop
- `$p-breakpoints-lg-up`: (min-width: 65em) - 1040px and above
- `$p-breakpoints-lg-down`: (max-width: 64.9975em) - Below 1040px
- `$p-breakpoints-lg-only`: (min-width: 65em) and (max-width: 89.9975em) - 1040-1440px

#### XL (Extra Large) - Large Desktop
- `$p-breakpoints-xl-up`: (min-width: 90em) - 1440px and above
- `$p-breakpoints-xl-down`: (max-width: 89.9975em) - Below 1440px
- `$p-breakpoints-xl-only`: (min-width: 90em) - 1440px and above (no upper limit)

## Mobile-First Strategy

Polaris recommends mobile-first responsive design:

1. **Write mobile styles first** (XS breakpoint)
2. **Add tablet styles at SM** (490px)
3. **Add larger tablet/desktop at MD** (768px)
4. **Add desktop at LG** (1040px)
5. **Add large desktop at XL** (1440px)

### Example Sass Structure
```scss
.container {
  /* Mobile styles (XS) */
  width: 100%;
  padding: var(--p-space-400);

  /* Tablet portrait (SM) */
  @media #{$p-breakpoints-sm-up} {
    width: 95%;
  }

  /* Tablet landscape (MD) */
  @media #{$p-breakpoints-md-up} {
    width: 90%;
    max-width: 700px;
  }

  /* Desktop (LG) */
  @media #{$p-breakpoints-lg-up} {
    width: 80%;
    max-width: 1000px;
  }

  /* Large desktop (XL) */
  @media #{$p-breakpoints-xl-up} {
    width: 70%;
    max-width: 1200px;
  }
}
```

## Container Sizing Guidelines

### By Breakpoint
- **XS**: Full width with padding
- **SM**: 95-100% width
- **MD**: 700-800px max-width
- **LG**: 1000-1100px max-width
- **XL**: 1200-1400px max-width

## Layout Grid Columns

Typical responsive grid layouts:
- **XS**: 1 column (100% width)
- **SM**: 1-2 columns
- **MD**: 2-3 columns
- **LG**: 3-4 columns
- **XL**: 4-6 columns
