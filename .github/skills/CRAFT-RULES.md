# Craft Rules — Universal Design Rules

Brand-agnostic craft knowledge. Each file in `craft/*.md` is a dense rulebook on one dimension of professional UI craft.

## How skills opt into craft rules

Add `od.craft.requires` in skill frontmatter:

```yaml
od:
  craft:
    requires: [typography, color, anti-ai-slop]
```

## Craft files

### typography
- **Scale:** Display 48-72px, H1 32-48px, H2 24-32px, H3 20-24px, Body 15-18px, Small 13-14px, Caption 11-12px
- **Line-height:** Display/H1 ≥32px → 1.0-1.2, Body 15-18px → 1.5-1.6, Small ≤14px → 1.5
- **Letter-spacing:** Body 0em, Small 0.01-0.02em, UI labels 0.02em, ALL CAPS 0.06-0.1em (required), Headings 32px+ -0.01 to -0.02em, Display 48px+ -0.02 to -0.03em
- **Font pairing:** Max 2 typefaces per artifact. Always declare system-ui fallback
- **Line length:** 50-75 characters per line (65ch default)
- **3-weight system:** Read 400/450, Emphasize 510/550, Announce 590/600
- **ALL CAPS without letter-spacing ≥0.06em is the most common AI-slop tell**

### typography-hierarchy
- **Core contract:** 1 dominant entry point, intentional rhythm, recoverable flow
- **5 vectors:** Scale, Weight, Spacing, Tracking, Alignment
- **Role inversions:** `<h1>` may be visually quieter than `<p>` — allowed
- **Failure modes:** Flat hierarchy (everything same visual weight), Noise hierarchy (too many competing elements)
- **3-level model:** Primary (entry point), Secondary (structure), Tertiary (incidental)
- **Anti-patterns:** Graduated weight ladder, uniform spacing, symmetrical emphasis, size-only hierarchy

### typography-hierarchy-editorial
- **Editorial extension:** Dramatic scale jumps (display 56-96px)
- Whitespace carries hierarchy, restrained bold, display tracking -0.02 to -0.05em
- Pull quotes as rhythm disruption, 60-70ch measure, asymmetrical rhythm

### color
- **Palette layers:** Neutrals 70-90%, Accent 5-10%, Semantic 0-5%, Effect <1%
- **Accent discipline:** Max 2 visible uses of `--accent` per screen. Links count as accent
- **Contrast minimums:** Body text 4.5:1, Large text 3:1, UI components 3:1
- **Dark themes:** Avoid pure black and pure white. `--bg: #0f0f0f`, `--fg: #f0f0f0`
- **Anti-defaults:** Indigo `#6366f1` = AI-slop tell, two-stop gradient = 2nd biggest tell
- **Always name tokens by purpose, never by hue:** `--accent` not `--blue-500`

### anti-ai-slop
- **7 cardinal sins (P0, auto-linted):**
  1. Tailwind indigo as accent (`#6366f1`, `#4f46e5`, etc.)
  2. Two-stop "trust" gradient on hero (purple→blue, blue→cyan)
  3. Emoji as feature icons (`✨`, `🚀`, `🎯`, `⚡`, `🔥`, `💡`)
  4. Sans-serif on display when seed binds serif
  5. Rounded card with colored left-border accent
  6. Invented metrics ("10× faster", "99.9% uptime")
  7. Filler copy (lorem ipsum, "feature one / two / three")
- **P1 soft tells:** Hero→Features→Pricing→FAQ→CTA sequence with no variation, external placeholder CDNs, `var(--accent)` used 6+ times
- **P2 polish tells:** No `data-od-id`, decorative blob/wave SVGs, perfect symmetric layout
- **Soul recipe:** ~80% proven patterns + ~20% distinctive choice

### accessibility-baseline
- **Legal floor:** WCAG 2.2 AA as working ceiling. EU EAA → WCAG 2.1, ADA Title II → WCAG 2.1, Section 508 → WCAG 2.0
- **Touch targets:** AA = 24×24 CSS px, AAA = 44×44 CSS px
- **Focus visibility:** `:focus-visible` for keyboard, `outline: none` is triple failure
- **Form labels:** `aria-describedby` as production default. 51% of top 1M home pages have missing labels (WebAIM 2026)
- **Keyboard operability:** Tab reachability, activation keys, no keyboard trap, focus order, native control first
- **ARIA discipline:** Native HTML → APG pattern → documented deviation. Never invent ARIA
- **Native mobile parity:** iOS UIKit/SwiftUI, Android Compose, Flutter, React Native — each has own labeling API

### animation-discipline
- **Duration thresholds:** 50ms-500ms+ based on context
- **Easing:** Overlap-on-tap, spring physics for entry
- **Reduced motion:** `prefers-reduced-motion` must be honored
- **Repeated/ambient motion:** Rules for continuous and background motion
- **Cross-platform:** iOS/Android/Web handoff parity

### state-coverage
- **5 required states:** Loading (skeleton/spinner + 15s fallback), Empty (headline + explanation + CTA), Error (cause + recovery + preserved input), Populated (the designed state), Edge (extreme volume, long strings, RTL)
- **3 form-specific states:** Untouched, Dirty (valid), Submitted-pending
- **Empty state composition:** First-use (illustration + headline + CTA), No-results (echo query + suggestions), Cleared (celebratory), Error-as-empty (never)
- **Error state:** What happened → Why → What user can do
- **Loading thresholds:** 0-300ms (none), 300ms-2s (skeleton), 2-10s (labeled spinner), 10-30s (determinate progress + cancel), 30-60s (progress + cancel), 60s+ (error + retry)
- **ARIA rules:** role="alert", role="status", role="alertdialog", focus management

### form-validation
- **Input state machine:** pristine → dirty → touched → invalid → recovering → submitting → server-error
- **Validation timing:** On blur, not on first keystroke
- **Constraint Validation API** + ARIA error wiring
- **WCAG 3.3.x:** Error identification, labels/instructions, error suggestion, prevention
- **Native mobile parity:** iOS/Android form validation patterns

### rtl-and-bidi
- **Base direction/language:** `dir="auto"`, `lang` attribute
- **Logical properties:** `margin-inline-start`, `padding-inline-end`, etc.
- **UAX #9:** Bidirectional text handling
- **What does not mirror:** Numbers, timelines, directional icons
- **Typography:** Arabic/Hebrew font rules, line-height, letter-spacing
- **Forms in RTL:** Label alignment, input direction, validation messages

### laws-of-ux
- **Perception/visual grouping:** Proximity, Similarity, Common Region, Prägnanz, Uniform Connectedness, Selective Attention, Von Restorff, Aesthetic-Usability
- **Decision-making:** Hick's Law (3-5 options), Choice Overload (3-4 tiers), Anchoring, Pareto 80-20, Tesler's Law, Occam's Razor
- **Memory/learning:** Miller's Law (4-7 chunks), Working Memory, Serial Position, Peak-End, Zeigarnik
- **Interaction/motor:** Fitts's Law, Doherty Threshold, Flow, Goal-Gradient, Postel's Law
- **Behavior/expectation:** Jakob's Law, Mental Model, Paradox of Active User, Parkinson's Law, Cognitive Load
