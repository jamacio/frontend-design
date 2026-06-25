# Frontend Design — Super Design Agent

A self-contained design workspace. This `.github/` directory holds **everything an AI agent needs** to generate production-grade frontends — craft rules, brand systems, tokens, templates, and 150+ functional skills. No external fetches. No cloud lock-in.

## How it works

```
1. Describe what you want — a landing page, a slide deck, a PDF report
2. The agent reads AGENTS.md → applies craft rules → picks templates → generates
3. Output lands in Projects/<name>/
```

## Example prompts

Try these with any AI agent connected to this workspace:

| You say… | What the agent does |
|---|---|
| *"Build a modern website from my Instagram profile — extract the brand colors, logo, and vibe, then generate a landing page with matching sections and imagery."* | Reads `project-generator` skill → extracts brand identity from Instagram → applies `imagegen-frontend-web` for section images → uses `saas-landing` or `kami-landing` template → outputs to `Projects/<name>/` |
| *"Create a website based on my Facebook page — pull the visual identity, generate a hero + features + testimonials layout, and ship a single HTML file."* | Reads `project-generator` skill → extracts brand from Facebook → runs `design-brief` to structure intent → uses `image-to-code` → picks `frontend-design-landing` or `saas-landing` → generates self-contained HTML |
| *"Turn this brand into a PDF report — use the brand's colors and fonts, 15 pages with cover, table of contents, and body sections."* | Reads `pdf` or `minimax-pdf` skill → applies brand tokens → generates a multi-page PDF with cover, typography, and layout from the design system |
| *"Make a slide deck / presentation for this project — Apple Keynote quality, horizontal swipe, with charts, quotes, and a closing CTA."* | Reads `ppt-keynote` or `frontend-slides` skill → uses `deck-swiss-international` or `kami-deck` template → generates a swipeable HTML deck or `.pptx` file |

## What's inside

### Design systems — 152 brands

```
design-systems/<brand>/
├── DESIGN.md           ← Full spec: theme, colors, typography, components, layout
├── tokens.css           ← CSS custom properties
├── tailwind-v4.css      ← Tailwind v4 @theme mapping
├── components.html      ← Live component fixtures
└── preview/             ← Visual preview pages
```

Apply any brand's identity in one step: `design-systems/airbnb/`, `design-systems/stripe/`, `design-systems/spotify/`, and 149 more.

### Craft rules — 10 universal standards

| Rule | What it enforces |
|---|---|
| `typography.md` | Scale, rhythm, hierarchy, measure |
| `color.md` | Palette roles, contrast, accessibility |
| `anti-ai-slop.md` | No generic gradients, no faux shadows, no stock UI |
| `accessibility-baseline.md` | WCAG 2.2 AA minimum |
| `animation-discipline.md` | Purposeful motion, reduced-motion respect |
| `state-coverage.md` | Hover, focus, active, disabled, error, empty |
| `form-validation.md` | Real-time feedback, error recovery |
| `rtl-and-bidi.md` | Bidirectional layout support |
| `laws-of-ux.md` | Jakob's Law, Hick's Law, Fitts's Law, etc. |

### Design templates — 112 rendering shapes

- **Prototypes:** `saas-landing/`, `dashboard/`, `blog-post/`, `pricing-page/`, `mobile-app/`, `kanban-board/`
- **Decks:** `html-ppt-*` (30+ themes), `kami-deck/`, `deck-swiss-international/`, `deck-guizang-editorial/`
- **PDFs:** `pdf/`, `minimax-pdf/`
- **Slides:** `slides/`, `pptx/`, `ppt-keynote/`, `frontend-slides/`

### Functional skills — 150+ capabilities

From image generation (`imagegen-frontend-web`, `fal-generate`) to design briefs, brand audits, copywriting, social cards, video templates, and device mockups.

## Agent workflow

```
1. Read AGENTS.md                       → understand the full structure
2. Read skills/DESIGN-SYSTEMS.md        → token architecture
3. Read skills/CRAFT-RULES.md            → design quality gates
4. Read skills/DESIGN-TEMPLATES.md       → pick a template
5. design-systems/<brand>/DESIGN.md      → apply brand identity
6. design-templates/<template>/SKILL.md  → render the artifact
7. Projects/<name>/                      → output lives here
```

## How any agent connects

Add this to your agent's instructions:

```
This project uses .github/ as its design knowledge base.
Read .github/AGENTS.md before any design work.
Craft rules at .github/craft/, skills at .github/skills/,
design systems at .github/design-systems/.
```
