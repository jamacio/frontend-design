# Themes & Tokens — Design Token System

## Web App Tokens (`apps/web/src/styles/tokens.css`)

The Frontend Design web app uses **Neutral Product Workspace** theme with rust/orange accent.

### Surface Palette

| Token | Light | Dark |
|---|---|---|
| `--bg` | `#faf9f7` | `#1a1916` |
| `--bg-panel` | `#fdfcfa` | `#24221e` |
| `--bg-subtle` | `#f4f5f7` | `#2d2b26` |
| `--bg-elevated` | `#fffefc` | `#33312b` |

### Border Tokens

| Token | Light | Dark |
|---|---|---|
| `--border` | `#e1e5eb` | `#3d3a35` |
| `--border-strong` | `#c9d0da` | `#57534a` |
| `--border-soft` | `#edf0f4` | `#2d2b26` |

### Text Tokens

| Token | Light | Dark |
|---|---|---|
| `--text` | `#1a1916` | `#f5f4f0` |
| `--text-muted` | `#74716b` | `#aba69b` |
| `--text-faint` | `#b3b0a8` | `#6b675c` |

### Accent

- **`--accent`:** `#c96442` (rust/orange)
- **`--selected`:** `#2563eb` (blue — independently controllable from accent)

### Semantic Colors

| Token | Color | Background |
|---|---|---|
| `--green` | OKLCh | Tinted bg |
| `--blue` | OKLCh | Tinted bg |
| `--purple` | OKLCh | Tinted bg |
| `--red` | OKLCh | Tinted bg |
| `--amber` | OKLCh | Tinted bg |

### Shadows

`--shadow-xs`, `--shadow-sm`, `--shadow-md`, `--shadow-lg` — with light and dark values.

### Radius Scale

| Token | Value |
|---|---|
| `--radius-xs` | 4px |
| `--radius-sm` | 6px |
| `--radius-base` | 8px |
| `--radius-md` | 10px |
| `--radius-lg` | 12px |
| `--radius-pill` | 999px |

### Motion Tokens

| Token | Value |
|---|---|
| `--ease-out` | `cubic-bezier(0.23, 1, 0.32, 1)` |
| `--dur-quick` | 120ms |
| `--dur-enter` | 200ms |
| `--dur-exit` | 140ms |

### Fonts

- **Serif:** Source Serif Pro
- **Sans:** system-ui, -apple-system, sans-serif
- **Mono:** ui-monospace, monospace

### Dark Mode

Activated via:
- `[data-theme="dark"]` — manual toggle
- `@media (prefers-color-scheme: dark)` — system preference

## Framer Motion Variants (`apps/web/src/motion.ts`)

- Spring physics for interactions
- Modal animations (scale-in)
- Toast slide-up
- List item stagger
- Popover in/out

## CSS Architecture

- `index.css` — Cascade entrypoint (31 imports)
- `base.css` — Minimal reset
- `primitives.css` — Global primitive classes (buttons, sr-only, accordion)
- CSS Modules (`.module.css`) — Component-scoped styles (preferred for new components)
- `packages/components/src/styles.css` — Shared component primitives

## Design System Tokens

Each design system in `design-systems/<brand>/` contains:

### tokens.css
Compiled CSS custom properties following the 4-layer schema.

### tailwind-v4.css
`@theme` mapping for Tailwind v4:

```css
@theme {
  --color-bg: var(--bg);
  --color-fg: var(--fg);
  --color-accent: var(--accent);
  --font-display: var(--font-display);
  --font-body: var(--font-body);
}
```

### design-tokens.json
Design Tokens JSON (W3C DTC format) derived from `tokens.css` + `source/token-contract.report.json`.

## UI Animation Philosophy

- **Default easing:** `cubic-bezier(0.23, 1, 0.32, 1)` — custom ease-out
- **Asymmetric durations:** Enter ~200ms, Exit ~140ms
- **Accordion:** `grid-template-rows: 0fr → 1fr` with opacity fade
- **Never animate from `transform: scale(0)`** — start from `scale(0.9)` with `opacity: 0`
- **Conditional elements:** Keep mounted and toggle CSS class (exit transitions need mounted elements)
