---
name: css-debug
description: Use ONLY when the user asks you to inspect, preview, debug, or adjust CSS, layout, responsiveness, or visual appearance of HTML pages or web projects. Trigger on keywords like "screenshot", "print", "capture", "layout", "CSS", "visual", "aparece", "render", "tela", "responsivo", "alinhamento", "quebrado", "quebrando", "não está aparecendo", "preview".
---

# CSS Debug — Playwright MCP

Use the Playwright tool server (`@playwright/mcp`) to take screenshots and inspect the visual rendering of HTML pages when the user reports CSS issues, layout problems, broken alignment, or responsiveness bugs.

## Workflow

1. **Start a browser session** — Use `playwright_navigate` to open the local HTML file (as `file:///absolute/path/to/file.html`).
2. **Set viewport** — Use `playwright_set_viewport` to match relevant breakpoints:
   - Desktop: `1920x1080` or `1440x900`
   - Tablet: `768x1024`
   - Mobile: `375x812` or `390x844`
3. **Take screenshots** — Use `playwright_screenshot` with `fullPage: true` to capture the full rendered page.
4. **Inspect elements** — Use `playwright_evaluate` to run JS that checks computed styles, dimensions, or layout issues:
   ```js
   // Example: check computed styles of an element
   const el = document.querySelector('.banner')
   getComputedStyle(el).background
   ```
5. **Show the user** — Share the screenshot(s) and explain what CSS changes are needed.
6. **Close the browser** — Use `playwright_close` when done.

## Always
- Set a descriptive viewport before screenshots
- Use `fullPage: true` unless a specific area is needed
- After making CSS edits, re-take a screenshot to confirm the fix
- Test responsive breakpoints when the issue mentions mobile/tablet

## Examples
- "essa tela está quebrada no mobile" → navigate, set 375x812, screenshot, inspect
- "o botão não aparece" → navigate, evaluate `document.querySelector('.btn').getBoundingClientRect()`
- "captura a página atual" → navigate, screenshot fullPage
