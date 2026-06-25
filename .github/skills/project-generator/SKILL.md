---
name: project-generator
description: |
  Mandatory rule for creating or modifying projects. Ensures Projects/ exists, creates new projects with subagents/skills, uses brand context from Instagram/Facebook, and fetches images from the web when generation is unavailable.
triggers:
  - "new project"
  - "create project"
  - "project"
  - "site from instagram"
  - "site from facebook"
  - "website from social media"
  - "criar projeto"
  - "novo projeto"
od:
  mode: utility
  category: project-management
  priority: P0
---

# project-generator

## MANDATORY RULES — Read before creating or modifying any project

### 1. Ensure Projects/ exists

1. Check the `Projects/` directory at the workspace root (relative to `.github/`: `../Projects/`).
2. If it **does NOT exist**, **CREATE IT IMMEDIATELY**: `mkdir -p ../Projects/`
3. If it exists, use it normally.

### 2. Create a new project inside Projects/

1. Create `Projects/<project-name>/`. If the user doesn't specify a name, invent one based on context.
2. Every new project **MUST** include subagents/skills:
   ```
   Projects/<project-name>/
   ├── skills/
   │   ├── <skill-1>.md
   │   └── <skill-2>.md
   └── agents/
       └── <agent-config>.md
   ```
3. Configure subagents and skills according to the project type.
4. Use `.github/` skills, craft rules, design systems, and templates as knowledge base.
5. Write all output (HTML, CSS, JS, assets, docs) inside `Projects/<project-name>/`.

### 3. Existing projects

1. If the user references an existing project in `Projects/`, **READ and MODIFY in-place**.
2. **NEVER** create a duplicate.
3. Read the project's current state before making changes.

### 4. Multiple projects

1. Projects live side-by-side: `Projects/site-a/`, `Projects/site-b/`, etc.
2. **NEVER** nest one project inside another.

### 5. Always read and understand first

1. Before creating or modifying ANY project:
   - Read `.github/AGENTS.md`, `.github/CONTEXT.md`
   - Understand what the user wants to create or modify
   - Check if a similar project already exists
   - Only then start executing

### 6. Add subagents and skills to every project

Every project created inside `Projects/` MUST include:
- A `skills/` folder with relevant skills
- An `agents/` folder with agent configuration

### 7. Sites from Instagram / Facebook — brand context

When the user asks to create a site from an **Instagram** or **Facebook** page:

1. **Extract brand identity** from the page/social media context:
   - Use the **logo**, brand colors, and visual style from the page
   - Match the brand's tone, typography, and messaging
   - Respect the existing brand identity
2. If the user provides a URL or page name, try to research it for:
   - Logo and branding assets
   - Color palette
   - Content, products, or services offered
   - Target audience
3. Apply all of this to the generated site.

### 8. Images — generation vs. web fetch

When generating a new site:

1. **Try to generate images first** using available image generation skills (`imagegen/`, `imagen/`, `venice-image-generate/`, `fal-generate/`, etc.).
2. If image generation is **not available or fails**, **fetch real images from the internet**:
   - Use `picsum.photos` with contextual seeds (e.g., `#`)
   - Use `unsplash.com` source URLs matching the project context
   - Search for relevant stock photos that match the brand/industry
   - Always pick images that match the content (e.g., bakery → bread/pastry photos, consulting → office/team photos)
3. **Never leave a site with empty or placeholder image slots.** Every image must have real content.

### 9. Example workflow

> **User:** "create a site from this Instagram page: @mybakery"
> **Agent:**
> 1. Checks `Projects/` exists
> 2. Creates `Projects/mybakery-site/` with skills/ and agents/
> 3. Researches @mybakery for logo, colors, style
> 4. Generates or fetches contextual images (bread, bakery interior)
> 5. Builds the complete site

> **User:** "update the control-consultoria site"
> **Agent:**
> 1. Checks `Projects/control-consultoria/` exists
> 2. Reads current files
> 3. Modifies in-place — no duplicates
