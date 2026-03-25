# Motion Tokens Reference

## Motion Easing Functions

### Linear
- `--p-motion-linear`: cubic-bezier(0, 0, 1, 1)
  - Constant speed throughout the animation
  - Use for: Continuous and mechanical animations (spinners, rotating elements)

### Ease (Default)
- `--p-motion-ease`: cubic-bezier(0.25, 0.1, 0.25, 1)
  - Responds quickly and finishes with control
  - Great default for any user interaction

### Ease In-Out
- `--p-motion-ease-in-out`: cubic-bezier(0.42, 0, 0.58, 1)
  - Starts and finishes with equal speed
  - Use for: Transitions triggered by the system

### Ease Out
- `--p-motion-ease-out`: cubic-bezier(0.19, 0.91, 0.38, 1)
  - Starts at top speed and finishes slowly
  - Use sparingly for specific emphasis effects

### Ease In
- `--p-motion-ease-in`: cubic-bezier(0.42, 0, 1, 1)
  - Starts slowly and finishes at top speed
  - Use sparingly for specific emphasis effects

## Motion Duration Tokens

### Instant
- `--p-motion-duration-0`: 0ms - No animation/instant

### Quick
- `--p-motion-duration-50`: 50ms - Very fast animations
- `--p-motion-duration-100`: 100ms - Fast animations

### Fast
- `--p-motion-duration-150`: 150ms - Fast feedback animations
- `--p-motion-duration-200`: 200ms - Standard quick transitions

### Standard
- `--p-motion-duration-250`: 250ms - Default animation duration
- `--p-motion-duration-300`: 300ms - Comfortable standard duration
- `--p-motion-duration-350`: 350ms - Slightly extended

### Moderate
- `--p-motion-duration-400`: 400ms - Moderate animation speed
- `--p-motion-duration-450`: 450ms - Slightly slower

### Standard-Extended
- `--p-motion-duration-500`: 500ms - Leisurely animation

### Slow
- `--p-motion-duration-5000`: 5000ms - Very slow animations (5 seconds)

## Motion Keyframes

### Bounce
- `--p-motion-keyframes-bounce`: Scales from 1 → 0.85 → 1.05 → 1
  - Use for: Attention-grabbing bounce effects
  - Keyframes at: from, 65%, 75%, 82.5%, 85%, to

### Fade In
- `--p-motion-keyframes-fade-in`: Transitions opacity from 0 to 1
  - Use for: Element entrance animations
  - Keyframes at: to

### Pulse
- `--p-motion-keyframes-pulse`: Scales 0.85 → 2.5 and fades from opaque to transparent
  - Use for: Attention-drawing pulse effects (badges, notifications)
  - Keyframes at: from/75%, to

### Spin
- `--p-motion-keyframes-spin`: Rotates element 1 full turn (360 degrees)
  - Use for: Loading spinners and rotating animations
  - Keyframes at: to

### Appear Above
- `--p-motion-keyframes-appear-above`: Slides up from --p-space-100 with fade-in
  - Use for: Dropdowns, popovers appearing from above
  - Transforms from: translateY(4px), opacity: 0

### Appear Below
- `--p-motion-keyframes-appear-below`: Slides down from --(p-space-100) with fade-in
  - Use for: Dropdowns, popovers appearing from below
  - Transforms from: translateY(-4px), opacity: 0

## Common Animation Patterns

### Quick Feedback (Buttons, Interactions)
```css
animation-duration: var(--p-motion-duration-200);
animation-timing-function: var(--p-motion-ease);
```

### Smooth Transitions
```css
transition-duration: var(--p-motion-duration-300);
transition-timing-function: var(--p-motion-ease);
```

### Entrance Animation
```css
animation: var(--p-motion-keyframes-fade-in);
animation-duration: var(--p-motion-duration-300);
animation-timing-function: var(--p-motion-ease-out);
```

### Loading Indicator
```css
animation: var(--p-motion-keyframes-spin);
animation-duration: var(--p-motion-duration-5000);
animation-timing-function: var(--p-motion-linear);
animation-iteration-count: infinite;
```

### Attention Effect
```css
animation: var(--p-motion-keyframes-pulse);
animation-duration: var(--p-motion-duration-1000);
animation-timing-function: var(--p-motion-ease-in-out);
```

## Motion Duration Guidelines

- **Feedback animations** (hover, click): 50-200ms
- **Standard transitions** (color, position): 200-300ms
- **Entrance/exit animations**: 300-400ms
- **Drawer/modal animations**: 300-500ms
- **Page transitions**: 300-400ms
- **Loading indicators**: 500-5000ms
- **User interactions**: Keep under 300ms for immediate feedback
