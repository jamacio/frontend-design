---
name: shadcn-ui
description: 'Build UI components with shadcn/ui. Pairs with the Stitch design loop
  to ship structured, accessible components quickly.

  '
triggers:
- shadcn
- shadcn ui
- shadcn components
- accessible components
od:
  mode: design-system
  category: design-systems
---
# shadcn/ui

## Overview
Builds UI components using shadcn/ui conventions — copy-paste components built on Radix UI primitives with Tailwind CSS styling. Focuses on composability, accessibility, and design system alignment.

## Conventions
- Components are installed via `npx shadcn@latest add <component>`
- Styling uses Tailwind CSS classes (not CSS modules)
- Components are accessible by default (Radix primitives)
- Dark mode via `class` strategy on `<html>` element
- Customizable via `tailwind.config.js` theme extension

## Workflow
1. Identify the needed component pattern
2. Install via shadcn CLI or copy manually from source
3. Customize colors, spacing, and typography to match active design system
4. Ensure proper composition — one component per file, shallow imports
5. Test keyboard navigation and screen reader behavior
