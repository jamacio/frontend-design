---
name: enhance-prompt
description: 'Improve prompts with design specs and UI/UX vocabulary. Useful for design-to-code
  workflows and clarifying requests for visual output.

  '
triggers:
- enhance prompt
- design prompt
- ui prompt
- design vocabulary
od:
  mode: design-system
  category: design-systems
---
# Enhance Prompt

## Overview
Transforms vague UI/design ideas into detailed, structured prompts optimized for AI design generation. Adds specificity, UI/UX vocabulary, design system context, and output format constraints.

## Enhancement Process

### Input analysis
- Identify the core ask (page, component, flow)
- Note any explicit constraints (platform, brand, audience)

### Enrichment layers
1. **Surface & platform**: desktop, mobile, tablet, responsive
2. **Atmosphere & tone**: professional, playful, minimalist, editorial
3. **Design system**: inject active tokens if available
4. **Layout structure**: sections, grid, density
5. **Component needs**: navigation, cards, forms, data display
6. **States**: loading, empty, error, edge cases
7. **Craft rules**: reference active craft constraints

## Output
A structured prompt ready for a design agent, with all contextual constraints baked in.
