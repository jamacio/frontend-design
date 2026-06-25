# Design Templates — Template Catalog

112 templates in `design-templates/` organized by rendering mode.

## By mode (`od.mode`)

### Prototype (~30+)
Functional prototype templates:
- `saas-landing/` — SaaS landing page with hero, features, social proof, pricing, CTA
- `dashboard/` — Admin/analytics dashboard with sidebar, KPI cards, charts
- `blog-post/` — Long-form article with masthead, hero, figures, pull quotes, byline
- `pricing-page/` — Pricing page with tiers
- `docs-page/` — Documentation page
- `waitlist-page/` — Waitlist page
- `mobile-app/` — Mobile application
- `web-prototype-*/` — Various web prototypes
- `kanban-board/` — Kanban board
- `contact-widget/` — Contact widget
- `wireframe-sketch/` — Wireframe sketch

### Deck (~50+)
Presentation templates:
- `html-ppt-pitch-deck/` — Pitch deck HTML
- `html-ppt-zhangzara-*/` — ~30+ Zhang Zara themes (8-bit orbit, biennale yellow, blue professional, bold poster, broadside, capsule, cartesian, cobalt grid, coral, creative mode, daisy days, editorial tri-tone, grove, long table, mat, monochrome, neo-grid-bold, peoples platform, pin and paper, pink script, playful, raw grid, retro windows, retro zine, sakura chroma, scatterbrain, signal, soft editorial, stencil tablet, studio, vellum)
- `html-ppt-taste-*/` — Taste themes (brutalist, editorial, testing/safety-alert)
- `kami-deck/` — Kami paper-style slide deck
- `replit-deck/` — Replit deck
- `ib-pitch-book/` — Investment banking pitch book
- `frontend-design-landing-deck/` — Frontend Design landing deck
- `guizang-ppt/` — Guizang theme

### Template (~10+)
Miscellaneous templates:
- `tweaks/` — Live side panel with parameterized controls (accent, type scale, density, motion, theme) — rewrites CSS vars in real time
- `social-carousel/` — Social media carousel
- `image-poster/` — Image poster
- `magazine-poster/` — Magazine poster

### Image (~5+)
- `image-poster/` — Static poster
- `magazine-poster/` — Editorial poster
- `sprite-animation/` — Sprite animation

### Video (~5+)
- `video-shortform/` — Short video
- `motion-frames/` — Motion frame sequences
- `hyperframes/` — HyperFrames motion graphics
- `audio-jingle/` — Audio jingle

### Audio (1)
- `audio-jingle/` — Audio jingle

## Craft requirements by template

| Template | Craft rules |
|---|---|
| `saas-landing/` | typography, color, anti-ai-slop, laws-of-ux |
| `dashboard/` | state-coverage, accessibility-baseline, laws-of-ux |
| `blog-post/` | typography, typography-hierarchy, typography-hierarchy-editorial, rtl-and-bidi |
| `tweaks/` | N/A (live tweaks panel) |
| `html-ppt-*/` | Varies by theme |
| `kami-deck/` | typography |
| `motion-frames/` | animation-discipline |
