# Frontend Design — Super Design Agent

Welcome to the self-contained design super agent. This `.github/` directory contains **all design knowledge, themes, craft rules, brands, and skills** from the Frontend Design project in a single place. Any AI agent (AI assistant, code agent, code editor, AI assistant, OpenCode, etc.) can use this directory as a complete reference for generating professional designs.

## How any agent uses this super agent

### 1. Entry point — `AGENTS.md`

Read `AGENTS.md` first. It contains the full index of everything available and navigation instructions.

### 2. Design skills — `skills/`

Five compact markdown documents with all essential knowledge:

| File | Contents |
|---|---|
| `skills/DESIGN-SYSTEMS.md` | 152 design system architecture, 4-layer token schema (A1-identity, A1-structure, A2, B-slot), promotion path, starters |
| `skills/CRAFT-RULES.md` | 10 universal craft rules: typography, color, anti-AI-slop, WCAG 2.2 AA accessibility, animation, states, forms, RTL, UX laws |
| `skills/THEMES-TOKENS.md` | Web app CSS themes (Neutral Product Workspace), dark mode, motion tokens, Tailwind v4, Framer Motion |
| `skills/DESIGN-TEMPLATES.md` | 112 template catalog organized by mode (prototype, deck, template, image, video, audio) |
| `skills/BRAND-REFERENCES.md` | Complete 152 brand catalog with palettes, typography, and characteristics |

### 3. Design systems — `design-systems/`

152 folders, each with:
- `DESIGN.md` — Canonical 9-section prose (theme, colors, typography, components, layout, depth, do's/don'ts, responsive, prompt guide)
- `tokens.css` — CSS custom properties
- `tailwind-v4.css` — Tailwind v4 @theme mapping
- `manifest.json` — Design system metadata
- `design-tokens.json` — Structured tokens (W3C DTC)
- `components.html` — Component fixture
- `preview/` — Color, typography, button, input previews

### 4. Craft rules — `craft/`

10 universal rule files any skill can require via `od.craft.requires`:
- `typography.md`, `typography-hierarchy.md`, `typography-hierarchy-editorial.md`
- `color.md`, `anti-ai-slop.md`
- `accessibility-baseline.md`, `animation-discipline.md`
- `state-coverage.md`, `form-validation.md`, `rtl-and-bidi.md`
- `laws-of-ux.md`

### 5. Design templates — `design-templates/`

112 renderable templates for prototypes, decks, images, videos, and audio. Each has `SKILL.md` with full instructions.

### 6. Functional skills — `skills/` (157 subfolders)

Complete skills with `SKILL.md`, ready for any agent compatible with the skills protocol.

## Agent workflow

```
1. Read AGENTS.md → understand the structure
2. Read skills/DESIGN-SYSTEMS.md → understand token architecture
3. Read skills/CRAFT-RULES.md → learn craft rules
4. For a specific brand: design-systems/<brand>/DESIGN.md
5. For a specific template: design-templates/<template>/SKILL.md
6. Apply craft/ rules on top of generated design
```

## License

Apache 2.0 — see `LICENSE`.
