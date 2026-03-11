# AGENTS.md

This file defines **coding standards, architecture rules, and generation guidelines** for AI agents working in this repository.

These instructions apply to:

* Cursor
* GitHub Copilot
* ChatGPT
* Devin
* Any automated coding agent

AI agents **must follow these rules strictly** when generating or modifying code.

---

# Project Overview

This project is a **static frontend website** built with:

* HTML
* CSS
* Vanilla JavaScript
* HTMX

The architecture uses **HTMX partial loading** to compose pages.

The project focuses on:

* semantic HTML
* BEM CSS architecture
* mobile-first layout
* minimal JavaScript
* predictable file structure

Do **not introduce frameworks** unless explicitly requested.

---

# Core Development Principles

AI-generated code must follow these principles:

1. Semantic HTML structure
2. HTMX partial architecture
3. Mobile-first responsive design
4. BEM CSS naming methodology
5. Minimal JavaScript
6. Predictable file structure
7. Semantic file naming
8. Simple Git workflow

Prefer **clarity and maintainability** over clever solutions.

---

# Project Structure

Typical repository structure:

```text
project-root
│
├── index.html
├── about-us.html
├── contacts.html
│
├── css
│   └── styles.css
│
├── js
│   └── script.js
│
├── img
│   ├── header
│   ├── products
│   ├── icons
│   └── backgrounds
│
└── partials
```

Rules:

* All images must be placed in `img/`
* JavaScript files must be placed in `js/`
* CSS must be placed in `css/`
* Partial HTML files should be stored in the project root or `partials/`

---

# File Naming Rules

All files must use **kebab-case**.

Correct:

```text
about-us.html
index.products-list.partial.html
global.header-nav.partial.html
```

Incorrect:

```text
aboutUs.html
ProductsList.html
headerNav.html
```

Do not use:

* camelCase
* PascalCase
* snake_case

---

# HTML Architecture

Pages should act as **containers for HTMX-loaded sections**.

Example page layout:

```html
<header class="header">
  <nav
    data-hx-trigger="load"
    data-hx-swap="outerHTML"
    data-hx-get="global.header-nav.partial.html"
  ></nav>

  <section
    data-hx-trigger="load"
    data-hx-swap="outerHTML"
    data-hx-get="index.hero.partial.html"
  ></section>
</header>

<main class="main">
  <section
    data-hx-trigger="load"
    data-hx-swap="outerHTML"
    data-hx-get="index.products-list.partial.html"
  ></section>
</main>

<footer
  data-hx-trigger="load"
  data-hx-swap="outerHTML"
  data-hx-get="global.footer.partial.html"
></footer>
```

Rules:

* Avoid large inline sections inside pages
* Use **HTMX partials instead**
* Each section should have **a dedicated partial**

---

# HTMX Partial Rules

Each page section should be implemented as a **separate partial**.

Naming format:

```text
[page].[component].partial.html
```

Examples:

```text
index.hero.partial.html
index.products-list.partial.html
index.testimonials.partial.html
contacts.map.partial.html
contacts.form.partial.html
```

Global components:

```text
global.header-nav.partial.html
global.footer.partial.html
```

Avoid generic names like:

```text
section.partial.html
block.partial.html
component.partial.html
```

---

# CSS Architecture

CSS must follow **BEM methodology**.

Pattern:

```text
.block
.block__element
.block__element--modifier
```

Example:

```text
.header
.header__nav
.header__menu
.header__menu-item
.header__menu-link
```

Example HTML:

```html
<nav class="header__nav">
  <ul class="header__menu">
    <li class="header__menu-item">
      <a class="header__menu-link" href="#">Home</a>
    </li>
  </ul>
</nav>
```

---

# Partial CSS Rules

Partial CSS files must contain **only local component or section styles**.

Rules:

* do not use selectors that can affect unrelated parts of the site
* do not style global HTML tags like `body`, `main`, `section`, `h1`, `h2`, `p`, `a`, `ul`, `ol`, `li` inside partial CSS files unless they are scoped by a local parent class
* do not place reset-like or site-wide rules in partial CSS files
* use `global.css` for global selectors, reset rules, typography defaults, utility classes, and site-wide layout rules
* scope selectors to the component or section namespace

Good:

```css
.products-list {
  padding: 20px;
}

.products-list__title {
  margin-bottom: 16px;
}
```

Also good:

```css
.products-list {
  h2 {
    margin-bottom: 16px;
  }
}
```

Bad:

```css
h2 {
  margin-bottom: 16px;
}

a {
  text-decoration: none;
}
```

---

# CSS Nesting

Prefer **CSS nesting** when it improves readability.

Rules:

* prefer nesting for styles that clearly belong to the same block
* keep nesting shallow and readable
* do not overnest selectors
* preserve clear BEM structure even when nesting is used

Example:

```css
.products-list {
  padding: 20px;

  .products-list__title {
    margin-bottom: 16px;
  }

  &:hover {
    color: var(--accent-color);
  }
}
```

---

# CSS Variables

All colors must be declared inside `:root`.

Example:

```css
:root {
  --color-primary: #7b1e3b;
  --color-dark: #1a1a1a;
  --color-light: #ffffff;
}
```

Usage:

```css
.button {
  background-color: var(--color-primary);
}
```

Avoid hardcoded colors when variables exist.

Do not use actual color value in variable name.

---

# Responsive Design

Use **mobile-first CSS**.

Example:

```css
.products {
  padding: 20px;
}

@media (min-width: 768px) {
  .products {
    padding: 40px;
  }
}
```

Avoid desktop-first media queries.

---

# Component Naming

Avoid generic class names.

Bad:

```text
.carousel
.slider
.tabs
.accordion
```

Good:

```text
.carousel-clients
.slider-products
.tabs-pricing
.accordion-faq
```

Each component should have a **unique namespace**.

You may use utility classes like `button` when needed.

---

# JavaScript Guidelines

JavaScript should be **minimal and modular**.

Rules:

* use vanilla JavaScript
* avoid large libraries
* avoid global variables
* prefer event delegation
* do not rely on `DOMContentLoaded` in this project
* because pages are composed with HTMX partials, initialize JavaScript after HTMX content finishes loading
* use dynamic `import()` for feature-specific scripts when appropriate

Recommended initialization pattern:

```javascript
function init() {
  import("...js");
}

const totalPartials = document.querySelectorAll(
  '[hx-trigger="load"], [data-hx-trigger="load"]'
).length;
let loadedPartialsCount = 0;

document.body.addEventListener("htmx:afterOnLoad", () => {
  loadedPartialsCount++;

  if (loadedPartialsCount === totalPartials) {
    init();
  }
});
```

Example:

```javascript
document.addEventListener("click", (event) => {
  if (event.target.matches(".accordion-faq__title")) {
    event.target.classList.toggle("accordion-faq__title--active");
  }
});
```

Avoid:

* `DOMContentLoaded` for partial-based page initialization
* jQuery
* large frameworks
* unnecessary DOM manipulation

---

# Accessibility

Always include:

* alt attributes
* semantic HTML
* readable color contrast

Example:

```html
<img src="img/products/red-wine.webp" alt="Bottle of red wine" />
```

---

# Performance Guidelines

Preferred image formats:

```text
webp
svg
```

Allowed image formats:

```text
jpg
jpeg
png
```

Avoid heavy unoptimized images.

---

# SVG Handling

SVG files are often used as ready-made assets.

Rules:

* do not enforce BEM classes inside SVG markup
* do not enforce semantic HTML attributes inside SVG markup
* only check that SVG code is valid and does not contain errors that make the file invalid
* keep SVG files usable as provided unless a specific edit is requested

---

# Code Formatting

Code should follow **Prettier formatting**.

Rules:

* 2 space indentation
* lowercase HTML attributes
* consistent nesting

Example:

```html
<section class="products">
  <h2 class="products__title">Our Products</h2>
</section>
```

---

# Git Workflow

This project uses a **very simple Git workflow**.

Allowed commands:

```bash
git add .
git commit -m "Semantic comment"
git pull
git push
```

Example workflow:

```bash
git add .
git commit -m "Add products list section"
git pull
git push
```

---

# Git Conflict Resolution

Use **merge mode**.

Configure once:

```bash
git config pull.rebase false
```

Do not use rebase.

---

# AI Code Generation Rules

When generating new code, AI agents must:

* follow BEM naming
* use mobile-first CSS
* prefer CSS nesting when it improves readability
* keep partial CSS locally scoped
* create HTMX partials for sections
* use semantic HTML
* use kebab-case filenames
* place images in `img/`
* avoid frameworks
* avoid unnecessary JavaScript
* initialize page scripts after HTMX partial loading, not with `DOMContentLoaded`

---

# Forbidden Patterns

AI agents must **not introduce**:

* React
* Vue
* Angular
* Tailwind
* jQuery
* CSS frameworks
* build systems
* bundlers

The project must remain **simple static HTML/CSS/JS**.

---

# When Editing Existing Code

AI agents must:

* preserve project structure
* respect existing naming conventions
* not rename files unnecessarily
* not remove HTMX logic
* keep code readable

---

# Summary

This repository follows a **strict but simple architecture**:

* HTMX partial based layout
* semantic HTML
* BEM CSS methodology
* mobile-first responsive design
* minimal JavaScript
* predictable file structure
* simple Git workflow

AI agents must follow these rules when generating or modifying code.
