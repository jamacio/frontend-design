---
name: php-site-generator
description: |
  Generate multi-page PHP websites with dynamic base URL routing, modular includes, and modern CSS/JS asset management. Prevents 404 errors from hardcoded paths by computing the base URL from $_SERVER['SCRIPT_NAME'].
triggers:
  - "php site"
  - "php website"
  - "site php"
  - "multi-page php"
  - "site em php"
  - "criar site php"
  - "pagina php"
  - "projeto php"
  - "php project"
  - "php static"
od:
  mode: utility
  category: project-management
  priority: P1
---

# php-site-generator

## Rules for creating PHP multi-page websites

### 1. Project location

1. All generated projects go into `Projects/<project-name>/` at the workspace root (`../Projects/` relative to `.github/`).
2. Create `Projects/` if it does not exist.
3. **Never** nest a project inside another project.
4. Each project is a standalone directory with its own `index.php` router.

### 2. Directory structure

```
Projects/<project-name>/
├── index.php              # Router — sets $baseUrl, includes header/page/footer
├── assets/
│   ├── css/
│   │   └── style.css      # All styles — animations, responsive, theme
│   └── js/
│       └── main.js        # Interactions — particles, scroll reveal, mobile menu
├── pages/
│   ├── home.php
│   ├── sobre.php           # (or about, services, contact — named per project)
│   └── ...
└── partials/
    ├── header.php          # <head>, nav, opening <main>
    └── footer.php          # closing </main>, footer, <script>
```

### 3. Dynamic base URL — prevent 404 on static assets

**Critical:** Define `$baseUrl` in `index.php` and use it in **every** URL reference across all partials and pages:

```php
// index.php — first line after <?php
$baseUrl = rtrim(dirname($_SERVER['SCRIPT_NAME']), '/\\');
```

This makes the site work from **any** server root:
- Running `php -S localhost:8080` from a parent directory → `$baseUrl = '/project-name'`
- Running `php -S localhost:8080` from inside the project directory → `$baseUrl = ''`

**Every** URL must use `<?= $baseUrl ?>`:

```html
<!-- header.php — assets -->
<link rel="stylesheet" href="<?= $baseUrl ?>/assets/css/style.css">
<script src="<?= $baseUrl ?>/assets/js/main.js"></script>

<!-- Navigation links -->
<a href="<?= $baseUrl ?>/">Home</a>
<a href="<?= $baseUrl ?>/?page=sobre">Sobre</a>

<!-- Form actions -->
<form action="<?= $baseUrl ?>/?page=contato" method="POST">

<!-- CTA and internal links in page content -->
<a href="<?= $baseUrl ?>/?page=contato" class="btn">Fale Conosco</a>
```

**Never** hardcode a subdirectory prefix like `/project-name/`. Use `<?= $baseUrl ?>` everywhere.

### 4. PHP router pattern

```php
<?php
$baseUrl = rtrim(dirname($_SERVER['SCRIPT_NAME']), '/\\');

$page = $_GET['page'] ?? 'home';

$allowed = ['home', 'about', 'services', 'contact'];
if (!in_array($page, $allowed)) {
  $page = 'home';
}

$currentPage = $page;

$pageTitle = ucfirst($page) . ' - Site Name';

include __DIR__ . '/partials/header.php';

$pageFile = __DIR__ . '/pages/' . $page . '.php';
if (file_exists($pageFile)) {
  include $pageFile;
} else {
  include __DIR__ . '/pages/home.php';
}

include __DIR__ . '/partials/footer.php';
```

### 5. Active nav state

Pass `$currentPage` from the router and use it in `header.php`:

```html
<a href="<?= $baseUrl ?>/" class="<?= $currentPage === 'home' ? 'active' : '' ?>">Home</a>
<a href="<?= $baseUrl ?>/?page=about" class="<?= $currentPage === 'about' ? 'active' : '' ?>">About</a>
```

### 6. JavaScript active nav

In `main.js`, use generic path detection without hardcoded subdirectory names:

```js
const currentPath = window.location.pathname + window.location.search;
document.querySelectorAll('.nav-links a').forEach(link => {
  const href = link.getAttribute('href');
  if (href && href !== '/' && currentPath.includes(href)) {
    link.classList.add('active');
  }
});
```

### 7. CSS/JS bundle structure

- Place **all** CSS in a single `style.css` file inside `assets/css/`.
- Place **all** JS in a single `main.js` file inside `assets/js/`.
- Use CSS custom properties (`:root`) for theme tokens (colors, fonts, spacing, transitions).
- Use `@keyframes` for animations and an `IntersectionObserver`-based `.reveal` class for scroll-triggered effects.
- Keep the JavaScript self-contained — no external dependencies.

### 8. Serving the site

```bash
# From the workspace root (if project is a subdirectory of another project):
php -S localhost:8080

# Or from the project root directly:
cd Projects/<project-name>
php -S localhost:8080
```

The `$baseUrl` approach ensures both methods work without 404 errors.

### 9. Design guidelines

- Use modern dark/custom themes (avoid Bootstrap defaults).
- Include animated background elements (floating orbs, particle systems).
- Implement scroll reveal animations.
- Use gradient accents and glassmorphism for depth.
- Make all pages responsive (mobile-first).
