# Polaris Selection & Input Components Skills

This directory contains 15 Shopify Polaris component skills for Selection and Input components. Each skill is a SKILL.md file with structured documentation extracted from the Polaris React documentation.

## Components

1. **Autocomplete** - Searchable input with dropdown suggestions
2. **Checkbox** - Multiple selection toggle
3. **Choice list** - Grouped radio buttons or checkboxes
4. **Color picker** - Visual color selection
5. **Combobox** - Accessible autocomplete input
6. **Date picker** - Visual calendar date selection
7. **Drop zone** - Drag-and-drop file upload
8. **Filters** - List/table filtering with query and filter options
9. **Form** - Form wrapper for submission handling
10. **Inline error** - In-context error messages
11. **Radio button** - Single choice selection
12. **Range slider** - Numeric value selection within range
13. **Select** - Classic dropdown menu
14. **Tag** - Interactive keywords for categorization
15. **Text field** - General-purpose text input

## Skill structure

Each SKILL.md file includes:

- **YAML frontmatter**: skill name and description with trigger words
- **When to use**: Usage guidance and context
- **Props**: Complete props table with types, defaults, and descriptions
- **Examples**: Working code examples from documentation
- **Best practices**: Do's and don'ts
- **Accessibility**: A11y patterns and keyboard support
- **Related components**: Links to related components

## File locations

```
polaris-skills/
└── components/
    └── selection-input/
        ├── autocomplete/SKILL.md
        ├── checkbox/SKILL.md
        ├── choice-list/SKILL.md
        ├── color-picker/SKILL.md
        ├── combobox/SKILL.md
        ├── date-picker/SKILL.md
        ├── drop-zone/SKILL.md
        ├── filters/SKILL.md
        ├── form/SKILL.md
        ├── inline-error/SKILL.md
        ├── radio-button/SKILL.md
        ├── range-slider/SKILL.md
        ├── select/SKILL.md
        ├── tag/SKILL.md
        ├── text-field/SKILL.md
        └── README.md
```

## Note on index-filters

The index-filters component (16th in the original list) returned a 404 from the Polaris documentation site, suggesting it may have been removed, renamed, or relocated. Only the 15 successfully documented components are included.

## Usage

These skills are designed to be used with Claude Code to provide comprehensive guidance on Polaris component implementation, props, accessibility patterns, and best practices.
