# Frontend Design

A local-first design workspace powered by AI agents. This project uses `.github/` as a self-contained design super agent — with craft rules, design systems, brand references, and templates — to generate frontend projects on demand.

## Structure

```
.github/          ← Design knowledge base for AI agents (craft, skills, brands, templates)
Projects/         ← AI-generated projects (sites, apps, components, artifacts)
```

## How it works

1. AI agents read `.github/AGENTS.md` to understand the design system.
2. Agents apply craft rules, brand palettes, and templates from `.github/`.
3. All generated output goes into `Projects/<project-name>/`.

Every file is local — no external API calls, no proprietary dependencies.
