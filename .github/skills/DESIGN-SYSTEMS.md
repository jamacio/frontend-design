# Design Systems — Architecture & Schema

## Project structure

Each modern design system follows this format:

```
design-systems/<slug>/
├── manifest.json              ← Project contract (v1)
├── DESIGN.md                  ← Canonical 9-section design prose for agents
├── tokens.css                 ← Compiled CSS custom properties
├── design-tokens.json         ← Design Tokens JSON (derived)
├── tailwind-v4.css            ← Tailwind v4 @theme mapping
├── components.html            ← Component fixture
├── components.manifest.json   ← Component inventory cache
├── preview/                   ← Static preview pages
│   └── *.html
├── source/                    ← Importer evidence
│   ├── evidence.md
│   ├── tokens.source.json
│   └── token-contract.report.json
└── assets/                    ← Brand assets (optional)
```

## 4-layer token schema

| Layer | Who decides | If omitted | Examples |
|---|---|---|---|
| **A1-identity** | Brand | Guard fails | `--bg`, `--fg`, `--accent`, `--font-display` |
| **A1-structure** | Brand | Guard fails | Type scale, `--container-max`, `--section-y-*` |
| **A2** | Brand (+ fallback) | Guard fails today | `--motion-fast`, `--success`, `--space-4`, `--font-mono` |
| **B-slot** | Brand or alias | Guard fails | `--fg-2 → var(--fg)`, `--surface-warm → var(--surface)` |

Brand-specific tokens outside the shared schema are `C-extensions` in allowlists.

## DESIGN.md format (9 sections)

1. **Visual Theme & Atmosphere** — Visual theme, mood, atmosphere
2. **Color Palette & Roles** — Color palette and roles
3. **Typography Rules** — Typography rules
4. **Component Stylings** — Component styling
5. **Layout Principles** — Layout principles
6. **Depth & Elevation** — Depth and elevation
7. **Do's and Don'ts** — What to do and not do
8. **Responsive Behavior** — Responsive behavior
9. **Agent Prompt Guide** — Prompt guide for agents

## Token promotion path

```
C-extension → B-slot → A2 → A1
(1 brand)   (≥2 brands, alias)  (≥2 brands, fallback)  (rare, identity)
```

## Starters

| System | Category | Key traits |
|---|---|---|
| **default/** | Neutral Modern | B2B, clean. Cobalt `#2F6FEB`. Inter sans-serif. 12-col grid, 1200px. |
| **warm-editorial/** | Warm Editorial | Serif-led, terracotta `#C0512F`. GT Sectra + Söhne. Off-white `#FAF7F2`. |
| **atelier-zero/** | Magazine collage | Paper `#efe7d2`, coral `#ed6f5c`. Inter Tight + Playfair Italic. |
| **kami/** | Editorial/Print | Parchment `#f5f4ed`, ink-blue `#1B365D`. Charter serif. No italics, no bold. |

## Imported brands (~72)

Apple, Linear, Notion, Stripe, Figma, Vercel, Airbnb, Nike, Spotify, BMW, Ferrari, Tesla, IBM, Nvidia, Shopify, Supabase, Sentry, MongoDB, PostHog, Cisco, Webex, Mastercard, Coinbase, Kraken, Binance, Revolut, Wise, Uber, Wired, The Verge, Xiaohongshu, Pinterest, PlayStation, SpaceX, Starbucks, Raycast, Superhuman, code editor, Warp, Framer, Miro, Airtable, Cal, Intercom, Resend, Zapier, AI assistant, Cohere, ElevenLabs, Minimax, AI provider AI, Ollama, Opencode AI, AI platform, RunwayML, Together AI, xAI, Expo, Lovable, Clay, Composio, HashiCorp, ClickHouse, Sanity, Webflow, among others.

## Design skill systems (~57)

brutalism, claymorphism, clean, colorful, contemporary, corporate, cosmic, creative, dashboard, dithered, doodle, dramatic, editorial, elegant, energetic, enterprise, expressive, fantasy, flat, friendly, futuristic, glassmorphism, gradient, hud, luxury, minimal, modern, mono, neon, neobrutalism, neumorphism, paper, perspective, premium, professional, publication, refined, retro, simple, sleek, spacious, storytelling, vibrant, vintage, agentic, bento, cafe, kami, gaming, thematic.
