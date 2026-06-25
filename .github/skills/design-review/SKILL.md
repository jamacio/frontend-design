---
name: design-review
description: 'Designer Who Codes: visual audit then fixes with atomic commits and
  before/after screenshots. Useful for tightening shipped UI before launch.

  '
triggers:
- design review
- visual audit
- before after
- pre launch design check
od:
  mode: design-system
  category: creative-direction
---
# Design Review

## Overview
Reviews UI designs and prototypes against craft rules, accessibility standards, design system compliance, and anti-AI-slop principles.

## Review Gates

### P0 — Must Pass
- No purple-to-pink gradients
- No generic emoji icons (📊 💡 🚀)
- No rounded card with left-border accent
- No Inter as display face
- No invented stats
- Color contrast meets WCAG AA minimum

### P1 — Should Pass  
- Consistent spacing rhythm (8px grid)
- Semantic heading hierarchy
- Meaningful placeholder content
- Responsive at target breakpoints

### P2 — Nice to Have
- Micro-interactions considered
- Loading states designed
- Empty states designed

## Workflow
1. Accept screenshot URL, component HTML, or design description
2. Check against P0 list — fail immediately if any hit
3. Run P1 and P2 checks, document each finding
4. Deliver structured review with severity, location, fix suggestion
