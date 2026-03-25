# Shopify Polaris React Component Skills

Complete skill library for Shopify Polaris design system components. Each skill provides documentation, props, examples, best practices, accessibility notes, and related components.

## Organization

### Images and Icons (5 skills)
- **Avatar** - Thumbnail representation of people/businesses
- **Icon** - Visual communication of actions and concepts
- **Keyboard Key** - Educate about keyboard shortcuts
- **Thumbnail** - Visual anchor for objects
- **Video Thumbnail** - Clickable video placeholder

### Feedback Indicators (10 skills)
- **Badge** - Status and tone indicators
- **Banner** - Important persistent messages
- **Exception List** - Standout information with context
- **Progress Bar** - Visual task completion
- **Skeleton Body Text** - Loading state for content
- **Skeleton Display Text** - Loading state for headings
- **Skeleton Page** - Full page loading layout
- **Skeleton Tabs** - Loading state for tabs
- **Skeleton Thumbnail** - Loading state for images
- **Spinner** - Loading indicator for operations

### Typography (1 skill)
- **Text** - Universal typography component (replaces DisplayText, Heading, Caption, TextStyle, VisuallyHidden)

### Tables (2 skills)
- **Data Table** - Structured data presentation for analysis
- **Index Table** - Collection display with selection, sorting, filtering, pagination

### Lists (7 skills)
- **Action List** - Actionable item lists
- **Description List** - Terms with definitions/descriptions
- **List** - Text-only related items
- **Listbox** - Interactive vertical option list
- **Option List** - Grouped selectable items
- **Resource Item** - Individual object representation
- **Resource List** - Collection of same-type objects

### Navigation (4 skills)
- **Footer Help** - Contextual help in page footer
- **Link** - Navigation element
- **Pagination** - Page navigation control
- **Tabs** - Content section switching

### Overlays (2 skills)
- **Popover** - Floating container for content
- **Tooltip** - Hover/focus help text

### Utilities (3 skills)
- **App Provider** - Root wrapper providing theme and configuration
- **Collapsible** - Expandable/collapsible content blocks
- **Scrollable** - Long-form content with scrolling

## Usage

Each component skill is structured as:
```
components/{category}/{component-name}/SKILL.md
```

### Skill File Format
Each SKILL.md contains:
- **YAML Frontmatter**: name, description, and trigger words
- **When to use**: Use cases and scenarios
- **Props**: All properties with types and defaults
- **Examples**: Code samples
- **Best practices**: Design and implementation guidance
- **Accessibility**: ARIA, keyboard support, screen reader notes
- **Related components**: Links to complementary components

## Key Features

- **Comprehensive** - All 34 components from Polaris React
- **Practical** - Focuses on implementation with real examples
- **Accessible** - Detailed accessibility guidance for each component
- **Updated** - Based on Polaris React documentation
- **Organized** - Grouped by category for easy discovery

## Quick Reference

### When to use Badge
Status indicators, progress states, fulfillment/payment status

### When to use Banner
Important persistent conditions, arch announcements, form errors (summary)

### When to use Tabs
Organize content into separate sections, switch between related views

### When to use IndexTable
Display collections (orders, products) with selection, sorting, filtering

### When to use Collapsible
Show/hide optional lower-priority information

### When to use Text
All typography needs - replaces older typography components

## Component Statistics
- **Total Components**: 34
- **Categories**: 8
- **Total Lines of Documentation**: 3000+
