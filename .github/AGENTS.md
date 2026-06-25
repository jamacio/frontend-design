# Frontend Design — Super Design Agent

This `.github/` directory is a self-contained super agent with all design knowledge, themes, craft rules, brand references, and skills. Everything is local — no external fetches and no proprietary AI brand names.

## Structure

```
.github/
├── AGENTS.md                 ← This file — super agent entry point
├── AGENT.md                  ← Alternate entry (@AGENTS.md)
├── CONTEXT.md                ← Additional context
├── README.md                 ← How any agent uses this super agent
├── LICENSE                   ← License
│
├── skills/                   ← SKILL DOCS + FUNCTIONAL SKILLS
│   ├── AGENTS.md             ← Skills guide
│   ├── BRAND-REFERENCES.md   ← 152 brand catalog
│   ├── CRAFT-RULES.md        ← Universal craft rules
│   ├── DESIGN-SYSTEMS.md     ← Design system architecture
│   ├── DESIGN-TEMPLATES.md   ← 112 design templates
│   ├── THEMES-TOKENS.md      ← Theme and token system
│   └── <155 skills/>         ← Functional skills (all have real content)
│
├── design-systems/           ← 152 DESIGN SYSTEMS (brands)
├── craft/                    ← 10 UNIVERSAL CRAFT RULES
├── design-templates/         ← 112 DESIGN TEMPLATES
├── docs/                     ← DOCUMENTATION
└── assets/                   ← STATIC ASSETS

Projects/                     ← AI-GENERATED PROJECTS (created on demand)
│                               All projects produced by any AI agent go here.
│                               This is the single output directory for every
│                               generated site, app, component, or artifact.
└── <project-name>/           ← One folder per project
```

## How to use

1. Read `AGENTS.md` first — understand the full structure
2. Read `skills/` docs — contains all design knowledge in markdown
3. Check `design-systems/<brand>/DESIGN.md` — specific brand design
4. Apply `craft/*.md` rules — universal craft knowledge over any design
5. Use `design-templates/<template>/SKILL.md` — base for rendering artifacts
6. Generated output goes into `Projects/<project-name>/` at the workspace root

## ⚠️ ABSOLUTE RULE — Project Generator

**Read `skills/project-generator/SKILL.md` before creating or modifying any project.** This skill contains mandatory rules. Summary below:

### Project generation — mandatory rules

1. **Projects/ MUST exist** — if `../Projects/` (relative to `.github/`) **does not exist, CREATE IT IMMEDIATELY** (`mkdir -p ../Projects/`).
2. **Create project** — create `../Projects/<project-name>/`. If no name given, invent one.
3. **EVERY new project MUST include subagents/skills** inside the project folder:
   ```
   Projects/<project-name>/
   ├── skills/          ← Project-relevant skills
   └── agents/          ← Subagent configuration
   ```
4. **Use `.github/` skills, craft rules, design systems, and templates** as knowledge base.
5. **Write all output** (HTML, CSS, JS, assets, docs) inside `../Projects/<project-name>/`.
6. **Multiple projects** live side-by-side: `Projects/site-a/`, `Projects/site-b/`, etc. **NEVER** nest one project inside another.
7. **Existing projects** — if user references a project already in `Projects/`, **READ and MODIFY in-place**. **NEVER** create duplicates.
8. **Always read and understand first** — before acting, read the existing project, context, and understand what the user wants.
9. **Instagram / Facebook context** — when creating a site from a social media page, extract the logo, brand colors, visual style, and content from the page context. Use them in the generated site.
10. **Images** — try to generate images first. If generation is unavailable, fetch real images from the web (`picsum.photos`, `unsplash`, etc.) matching the project context. Never leave empty image placeholders.

All decisions (layout, typography, color, motion, brand alignment) are informed by the craft rules and design systems in this `.github/` directory.

## Notes

- All skills are self-contained — no stubs, no external fetches required.
- All AI brand names have been replaced with generic equivalents (AI provider, AI model, AI assistant, etc.).
- The agent never leaves the local filesystem.

## Quick links

- **Design system architecture:** `skills/DESIGN-SYSTEMS.md`
- **Craft rules summary:** `skills/CRAFT-RULES.md`
- **Theme & tokens:** `skills/THEMES-TOKENS.md`
- **Template catalog:** `skills/DESIGN-TEMPLATES.md`
- **Brand catalog:** `skills/BRAND-REFERENCES.md`
- **Brand palette:** `design-systems/<brand>/tokens.css`
- **Brand full design:** `design-systems/<brand>/DESIGN.md`
- **Typography rules:** `craft/typography.md`
- **Color rules:** `craft/color.md`
- **Anti-AI-slop (P0):** `craft/anti-ai-slop.md`
- **Accessibility:** `craft/accessibility-baseline.md`
- **Landing template:** `design-templates/saas-landing/SKILL.md`
- **Dashboard template:** `design-templates/dashboard/SKILL.md`
