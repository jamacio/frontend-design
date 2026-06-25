---
name: web-design-guidelines
description: 'Web design guidelines and standards by the Vercel engineering team.
  Covers layout, typography, color, motion, and accessibility for product UI.

  '
triggers:
- web design guidelines
- vercel design
- product ui standards
- design checklist
od:
  mode: design-system
  category: design-systems
---
# Web Design Guidelines

## Overview
Web design best practices and conventions for modern web applications. Covers layout, performance, accessibility, and responsive design patterns.

## Core Principles

### Layout
- Use CSS Grid for page-level layout, Flexbox for component-level
- Establish a consistent spacing scale (4px or 8px base)
- Max content width: 1200px (desktop), 100% (mobile)
- Sidebar: 240-280px wide on desktop, hidden on mobile

### Performance
- LCP target: < 2.5s
- Avoid layout shifts — set dimensions on images
- Lazy load below-fold content
- Minimize main-thread work

### Accessibility
- All interactive elements reachable via keyboard
- Visible focus indicators
- Color not the only differentiator
- Touch targets ≥ 44px

### Responsive
- Mobile-first CSS
- Breakpoints: 640px, 768px, 1024px, 1280px
- Test at actual device sizes, not just Chrome DevTools
