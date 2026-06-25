---
name: design-md
description: 'Create and manage DESIGN.md files. Useful for capturing design direction,
  tokens, and visual rules in a single source of truth.

  '
triggers:
- design.md
- design doc
- design tokens doc
- visual rules doc
od:
  mode: design-system
  category: design-systems
---
# Design MD

## Overview
Creates and manages DESIGN.md files documenting design systems with color palettes, typography rules, component styles, layout principles, and agent prompt guides in the standardized 9-section format.

## Process

1. **Scan** the existing project for visual patterns, token files, and component usage
2. **Extract** colors, fonts, spacing, and component styles from source
3. **Structure** into the canonical DESIGN.md sections:
   - Visual Theme & Atmosphere
   - Color Palette & Roles
   - Typography Rules
   - Component Stylings
   - Layout Principles
   - Depth & Elevation
   - Do's and Don'ts
   - Responsive Behavior
   - Agent Prompt Guide
4. **Write** each section with concrete values and usage rules
5. **Validate** that every token maps to a real value found in the project
6. **Cross-reference** with the active design system for consistency

## Output
A complete DESIGN.md that an AI agent can use to generate new UI that matches the project's design language.
