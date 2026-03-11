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

```id="s11"
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

```id="s12"
about-us.html
index.products-list.partial.html
global.header-nav.partial.html
```

Incorrect:

```id="s13"
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

```html id="s14"
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

```id="s15"
[page].[component].partial.html
```

Examples:

```id="s16"
index.hero.partial.html
index.products-list.partial.html
index.testimonials.partial.html
contacts.map.partial.html
contacts.form.partial.html
```

Global components:

```id="s17"
global.header-nav.partial.html
global.footer.partial.html
```

Avoid generic names like:

```id="s18"
section.partial.html
block.partial.html
component.partial.html
```

---

# CSS Architecture

CSS must follow **BEM methodology**.

Pattern:

```id="s19"
.block
.block__element
.block__element--modifier
```

Example:

```id="s20"
.header
.header__nav
.header__menu
.header__menu-item
.header__menu-link
```

Example HTML:

```html id="s21"
<nav class="header__nav">
  <ul class="header__menu">
    <li class="header__menu-item">
      <a class="header__menu-link" href="#">Home</a>
    </li>
  </ul>
</nav>
```

---

# CSS Variables

All colors must be declared inside `:root`.

Example:

```css id="s22"
:root {
  --color-primary: #7b1e3b;
  --color-dark: #1a1a1a;
  --color-light: #ffffff;
  --color-gray: #f5f5f5;
}
```

Usage:

```css id="s23"
.button {
  background-color: var(--color-primary);
}
```

Avoid hardcoded colors when variables exist.

---

# Responsive Design

Use **mobile-first CSS**.

Example:

```css id="s24"
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

```id="s25"
.carousel
.slider
.tabs
.accordion
```

Good:

```id="s26"
carousel-clients
slider-products
tabs-pricing
accordion-faq
```

Each component should have a **unique namespace**.

---

# JavaScript Guidelines

JavaScript should be **minimal and modular**.

Rules:

* use vanilla JavaScript
* avoid large libraries
* avoid global variables
* prefer event delegation

Example:

```javascript id="s27"
document.addEventListener("click", (event) => {
  if (event.target.matches(".accordion-faq__title")) {
    event.target.classList.toggle("accordion-faq__title--active");
  }
});
```

Avoid:

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

```html id="s28"
<img src="img/products/red-wine.webp" alt="Bottle of red wine" />
```

---

# Performance Guidelines

Preferred image formats:



```id="s29"
webp
svg
```

Avoid heavy images but in general PNG and JPG image formats are allowed:

```id="s30"
large jpg
large png
```

Images should be optimized.

---

# Code Formatting

Code should follow **Prettier formatting**.

Rules:

* 2 space indentation
* lowercase HTML attributes
* consistent nesting

Example:

```html id="s31"
<section class="products">
  <h2 class="products__title">Our Products</h2>
</section>
```

---

# Git Workflow

This project uses a **very simple Git workflow**.

Allowed commands:

```id="s32"
git add .
git commit -m "Semantic comment"
git pull
git push
```

Example workflow:

```id="s33"
git add .
git commit -m "Add products list section"
git pull
git push
```

---

# Git Conflict Resolution

Use **merge mode**.

Configure once:

```id="s34"
git config pull.rebase false
```

Do not use rebase.

---

# AI Code Generation Rules

When generating new code, AI agents must:

* follow BEM naming
* use mobile-first CSS
* create HTMX partials for sections
* use semantic HTML
* use kebab-case filenames
* place images in `img/`
* avoid frameworks
* avoid unnecessary JavaScript

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
