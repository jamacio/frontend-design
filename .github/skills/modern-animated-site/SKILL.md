---
name: modern-animated-site
description: |
  Build modern, production-grade animated websites with scroll-triggered
  bidirectional animations, layered background images with proper opacity,
  card hover effects, parallax orbs, particle systems, and restrained
  motion design. Use for landing pages, portfolio sites, marketing
  pages, and any frontend that needs tasteful scroll animation and
  atmospheric depth.
triggers:
  - "scroll animation"
  - "modern website"
  - "animated landing page"
  - "scroll reveal"
  - "intersection observer"
  - "background image overlay"
  - "ken burns effect"
  - "parallax orbs"
  - "hover effect"
  - "motion design"
  - "animated site"
od:
  mode: prototype
  surface: web
  category: animation-motion
  source: "Production animated site patterns from control-consultoria project"
  craft:
    requires:
      - animation-discipline
      - accessibility-baseline
      - typography
      - color
---
# Modern Animated Site

Use this skill to add scroll-driven animations, atmospheric backgrounds, hover effects, and motion polish to any HTML/CSS/JS or framework-based website.

## Principles

1. **Motion must serve the content.** Animate to clarify hierarchy, guide attention, or add delight — never for its own sake.
2. **Replay on scroll-back.** Users scroll up and down. Animations must trigger every time an element enters the viewport, not only once.
3. **Backgrounds need depth without distraction.** Use layered images + gradient overlays at controlled opacities so the content stays readable.
4. **One motion language.** Consistent easing, duration, and animation types across the entire page.
5. **Respect reduced motion.** Always provide `prefers-reduced-motion` fallbacks.

---

## 1. Scroll Animation System (Intersection Observer)

### HTML Setup

Add `data-anim` and optional `data-delay` attributes to elements:

```html
<div class="card" data-anim="slide-up" data-delay="0">
<div class="card" data-anim="slide-up" data-delay="150">
<div class="card" data-anim="zoom-in" data-delay="300">
```

Animation types:
- `fade-in` — opacity 0 → 1
- `slide-up` — translateY(60px) → translateY(0) + fade
- `slide-left` — translateX(60px) → translateX(0) + fade
- `slide-right` — translateX(-60px) → translateX(0) + fade
- `zoom-in` — scale(0.8) → scale(1) + fade
- `scale-in` — scale(0.95) → scale(1) + fade

### CSS Foundation

```css
[data-anim] {
  opacity: 0;
  transition: opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1),
              transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

[data-anim].anim-visible {
  opacity: 1;
  transform: none;
}

[data-anim="slide-up"] { transform: translateY(60px); }
[data-anim="slide-left"] { transform: translateX(60px); }
[data-anim="slide-right"] { transform: translateX(-60px); }
[data-anim="zoom-in"] { transform: scale(0.8); }
[data-anim="scale-in"] { transform: scale(0.95); }
[data-anim="fade-in"] { transform: none; }
```

### Bidirectional JavaScript (Replay on Scroll-Back)

```javascript
function initAnimations() {
  const els = document.querySelectorAll('[data-anim]');
  if (!els.length) return;

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      const el = entry.target;
      const type = el.dataset.anim;
      if (entry.isIntersecting) {
        const delay = parseInt(el.dataset.delay) || 0;
        setTimeout(() => {
          el.classList.add('anim-visible');
        }, delay);
      } else {
        el.classList.remove('anim-visible');
        el.classList.remove('anim-' + type);
      }
    });
  }, { threshold: 0.15, rootMargin: '0px 0px -40px 0px' });

  els.forEach(el => observer.observe(el));
}
```

### Key details
- **Do NOT call `observer.unobserve(el)`** — that prevents replay on scroll-back.
- Remove animation classes on leave, re-add on enter with delay.
- Use `cubic-bezier(0.16, 1, 0.3, 1)` for smooth, slightly bouncy easing.
- Stagger delays: 0, 100-150, 200-300 for groups of 3+ elements.

---

## 2. Background Image System

### Structure

Every section with a background image uses three layers:

```html
<section class="about-section">
  <!-- Layer 1: Image -->
  <div class="about-bg-image"></div>
  <!-- Layer 2: Gradient overlay (controls how much image shows) -->
  <div class="about-bg-overlay"></div>
  <!-- Layer 3: Content -->
  <div class="container">...</div>
</section>
```

### CSS Pattern

```css
.section-bg-image {
  position: absolute;
  inset: 0;
  background: url('../images/bg.jpg') center/cover;
  opacity: 0.35;        /* controls image visibility */
  pointer-events: none;
}

.section-bg-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg,
    var(--bg) 0%,
    rgba(55, 54, 112, 0.25) 50%,
    var(--bg) 100%
  );
  pointer-events: none;
}
```

### Opacity Guidelines

| Section type | Image opacity | Overlay opacity |
|---|---|---|
| Hero (full-screen) | — (use gradient on bg) | 0.65–0.85 |
| Content sections | 0.30–0.45 | 0.20–0.30 |
| CTA sections | 0.40–0.50 | 0.75–0.85 |
| Cards with bg images | 0.12–0.25 (0.30–0.40 on hover) | — |

Always adjust so content text remains readable. The image should be felt, not competing.

### Ken Burns Effect (Banner/Slider)

```css
.banner-bg {
  animation: kenBurns 20s ease-in-out infinite alternate;
}

@keyframes kenBurns {
  0% { transform: scale(1) translateX(0); }
  100% { transform: scale(1.15) translateX(-30px); }
}
```

---

## 3. Card Design Patterns

### Service/Benefit Cards with Hover

```html
<div class="service-card" data-anim="zoom-in" data-delay="0" data-tilt>
  <div class="service-card-inner">
    <div class="service-card-bg" style="background-image: url(...)"></div>
    <div class="service-card-bg-overlay"></div>
    <div class="service-card-content">
      <h3>Title</h3>
      <p>Description</p>
      <a href="#" class="service-link">Link <i class="fas fa-arrow-right"></i></a>
    </div>
  </div>
</div>
```

```css
.service-card-inner {
  position: relative;
  padding: 36px 28px;
  overflow: hidden;
}

.service-card-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0.12;    /* subtle on default */
  transition: opacity 0.6s ease, transform 0.6s ease;
  transform: scale(1);
  z-index: 0;
}

.service-card:hover .service-card-bg {
  opacity: 0.3;      /* more visible on hover */
  transform: scale(1.05);  /* subtle zoom */
}

.service-card-content {
  position: relative;
  z-index: 2;
}
```

### Card Glow Effect

```css
.card-glow {
  position: absolute;
  top: -60%;
  left: 50%;
  transform: translateX(-50%);
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, rgba(135, 213, 103, 0.06), transparent);
  border-radius: 50%;
  pointer-events: none;
  transition: all 0.6s ease;
}

.card:hover .card-glow {
  transform: translateX(-50%) scale(1.5);
}
```

---

## 4. Parallax Orbs (Atmospheric Depth)

Large colored radial gradients that move subtly on scroll for ambient depth.

### HTML

```html
<div class="parallax-orb parallax-orb--1"></div>
<div class="parallax-orb parallax-orb--2"></div>
```

### CSS

```css
.parallax-orb {
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
  will-change: transform;
}

.parallax-orb--1 {
  top: 5%;
  left: -120px;
  width: 300px;
  height: 300px;
  background: radial-gradient(circle, rgba(135, 213, 103, 0.12), transparent 70%);
}

.parallax-orb--2 {
  bottom: 10%;
  right: -80px;
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, rgba(55, 54, 112, 0.18), transparent 70%);
}
```

### JavaScript (scroll-driven movement)

```javascript
function initParallaxOrbs() {
  const orbs = document.querySelectorAll('.parallax-orb');
  if (!orbs.length) return;

  window.addEventListener('scroll', () => {
    const scrollY = window.scrollY;
    orbs.forEach((orb, i) => {
      const speed = 0.02 * (i + 1);
      orb.style.transform = `translateY(${scrollY * speed}px)`;
    });
  }, { passive: true });
}
```

---

## 5. Floating Shapes

Animated border circles that drift for visual interest in banners/sections.

```css
.banner-shape {
  position: absolute;
  border: 2px solid rgba(135, 213, 103, 0.15);
  border-radius: 50%;
  background: rgba(135, 213, 103, 0.03);
}

.banner-shape--1 {
  width: 60px;
  height: 60px;
  top: 15%;
  left: 8%;
  animation: shapeDrift 18s ease-in-out infinite;
}

@keyframes shapeDrift {
  0%, 100% { transform: translate(0, 0) rotate(0deg); }
  25% { transform: translate(40px, -30px) rotate(90deg); }
  50% { transform: translate(-20px, -60px) rotate(180deg); }
  75% { transform: translate(30px, 20px) rotate(270deg); }
}
```

---

## 6. Particle System (Hero)

```css
.particle {
  position: absolute;
  width: 4px;
  height: 4px;
  background: var(--secondary);
  border-radius: 50%;
  opacity: 0;
  animation: particleFloat linear infinite;
}

@keyframes particleFloat {
  0% { transform: translateY(100vh) scale(0); opacity: 0; }
  10% { opacity: 0.6; }
  90% { opacity: 0.6; }
  100% { transform: translateY(-10vh) scale(1); opacity: 0; }
}
```

Generate particles in JS:

```javascript
function initParticles() {
  const container = document.querySelector('.hero-particles');
  for (let i = 0; i < 30; i++) {
    const p = document.createElement('div');
    p.className = 'particle';
    p.style.left = Math.random() * 100 + '%';
    p.style.animationDuration = (12 + Math.random() * 18) + 's';
    p.style.animationDelay = Math.random() * 20 + 's';
    p.style.width = p.style.height = (2 + Math.random() * 3) + 'px';
    container.appendChild(p);
  }
}
```

---

## 7. Hero Section Pattern

```css
.hero-bg {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(135deg,
      rgba(26, 26, 46, 0.85) 0%,
      rgba(55, 54, 112, 0.6) 50%,
      rgba(26, 26, 46, 0.85) 100%
    ),
    url('../images/hero-bg.jpg') center/cover fixed;
  pointer-events: none;
}
```

The hero overlay sits on top of the background:
- Dark gradient controls readability of text on top
- The accent color gradient (green/teal at 0.06-0.08) adds a subtle color cast
- No separate `.hero-bg-image` needed — the image is part of the `background` shorthand on the pseudo-element

---

## 8. Button Pulse Animation

```css
.btn-pulse {
  animation: btnPulse 2.5s ease-in-out infinite;
}

@keyframes btnPulse {
  0%, 100% { box-shadow: 0 0 0 0 rgba(135, 213, 103, 0.5); }
  50% { box-shadow: 0 0 0 16px rgba(135, 213, 103, 0); }
}
```

---

## 9. Icon Strategy

- **Decorative/visual icons** (like Phosphor `ph-fill`) — prefer to remove for a cleaner layout. Modern design reads better without unnecessary icon decoration.
- **Utility icons** (arrows, checkmarks, brand logos) — keep. Use Font Awesome `fas` for arrows/checks, `fab` for social brands.
- **Icon containers** — when removing icons, also remove the empty wrapper divs and their associated CSS to avoid layout gaps.
- If icons are desired, prefer Phosphor (lighter, more modern) over Font Awesome for visual icons. But consider whether icons genuinely add value at each location.

---

## 10. Performance Checklist

- [ ] Prefer `transform` and `opacity` animations only (GPU-composited)
- [ ] Use `will-change` on elements that animate continuously (orbs, parallax)
- [ ] Add `pointer-events: none` on decorative layers (bg images, overlays, orbs, shapes)
- [ ] `{ passive: true }` on scroll event listeners
- [ ] Load images at appropriate resolution (1920px wide max)
- [ ] Use `image-set` or `srcset` for responsive images where possible
- [ ] Avoid animating `width`, `height`, `top`, `left`, `margin`, `padding`

---

## 11. Responsive / Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  [data-anim] {
    opacity: 1 !important;
    transform: none !important;
    transition: none !important;
  }
  .particle { display: none; }
  .parallax-orb { display: none; }
  .banner-shape { display: none; }
  .btn-pulse { animation: none; }
  .banner-bg { animation: none; }
}

@media (max-width: 768px) {
  [data-anim] {
    transition-duration: 0.5s !important;
  }
  .parallax-orb { display: none; }
  .hero { padding: 100px 0 40px; }
}
```

---

## Directory / Project Structure

For a PHP static site with animations:

```
project/
├── index.php                    ← Router
├── pages/
│   ├── home.php                 ← Home page with animations
│   └── ...                      ← Other pages
├── partials/
│   ├── header.php
│   └── footer.php
├── assets/
│   ├── css/
│   │   └── style.css            ← All styles (~3000+ lines)
│   ├── js/
│   │   └── main.js              ← All JS (~200+ lines)
│   └── images/
│       ├── hero-bg.jpg
│       ├── about-bg.jpg
│       └── ...                  ← All local bg images
└── .htaccess                    ← Apache rewrite rules
```

Base URL pattern:
```php
<?php $baseUrl = rtrim(dirname($_SERVER['SCRIPT_NAME']), '/\\'); ?>
```

---

## Putting It All Together

The full animation pipeline for a modern page:

1. **Hero** — full-viewport with gradient overlay + bg image, particles, scroll-down indicator, entrance animation on content
2. **Section backgrounds** — `bg-image` + `bg-overlay` pattern for depth
3. **Scroll animations** — `data-anim` on cards/text/images with bidirectional Intersection Observer
4. **Parallax orbs** — floating radial gradients that drift on scroll
5. **Card hover effects** — bg image zoom + glow + border highlight
6. **Banner** — Ken Burns background + floating shapes + CTA
7. **All images local** — no external HTTP requests for background images
