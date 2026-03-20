# Tiny Toolset — Architecture Guide

> **Audience:** AI agents and developers who need to understand the codebase to add features or fix bugs.

---

## High-Level Overview

Tiny Toolset is a **zero-build, static-file** collection of browser-based calculators. Each tool lives in its own directory as a **single self-contained `index.html` file** that includes all HTML, CSS (via `<style>`), and JavaScript (via `<script>`).

There is **no build system, no bundler, no package.json, and no npm dependencies**. External libraries are loaded via CDN `<script>` tags.

---

## Project Structure

```
tinytoolset/
├── index.html                        # HOME PAGE — tool directory / launcher
├── home-affordability/
│   └── index.html                    # Tool: Home Loan EMI Calculator
├── days-between/
│   └── index.html                    # Tool: Days Between Two Dates
├── days-calculator/
│   └── index.html                    # Tool: Add/Subtract Days from a Date
├── income-tax-calculator/
│   └── index.html                    # Tool: India New Regime Income Tax
├── .agent/                           # Agent & contributor docs (this folder)
│   ├── ARCHITECTURE.md               # ← You are here
│   ├── DESIGN_SYSTEM.md              # Tokens, components, conventions
│   └── workflows/
│       └── add-new-tool.md           # Step-by-step guide to add a tool
└── README.md
```

---

## Key Architectural Decisions

### 1. Single-file tools
Each tool is a **standalone `index.html`** file. All CSS is in a `<style>` tag in the `<head>` and all JS is in a `<script>` tag at the end of `<body>`. This keeps deployment dead-simple (just static files) and avoids cross-dependency issues.

### 2. Alpine.js for interactivity
All tools use [Alpine.js 3.x](https://alpinejs.dev/) loaded via CDN:
```html
<script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
```
- The main Alpine component function is declared as a global function in the `<script>` block.
- It is bound to `<body>` via `x-data="componentName()"`.
- State, computed getters, and methods all live inside the returned object.

### 3. No shared CSS file
CSS custom properties (design tokens) are **duplicated** in each tool's `:root` block. This is intentional — each file is self-contained. When updating design tokens, **all files must be updated together** (see Design System doc for the canonical token list).

### 4. External libraries (CDN only)
| Library | Used In | CDN URL |
|---------|---------|---------|
| Alpine.js 3.x | All tools | `https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js` |
| Chart.js 4.x | Income Tax Calculator | `https://cdn.jsdelivr.net/npm/chart.js@4.4.2/dist/chart.umd.min.js` |

### 5. Home page (`index.html` at root)
The root `index.html` is the launcher / directory page. It lists all tools as clickable cards. It does **NOT** use Alpine.js— it is purely static HTML + CSS with entrance animations.

When a new tool is added, a corresponding card **must** be added to this page.

---

## File Anatomy — Tool Template

Every tool file follows this layout:

```
<!DOCTYPE html>
<html lang="en">
<head>
  ├── <meta charset, viewport>
  ├── <title> — descriptive, includes tool name
  ├── <meta name="description"> — SEO summary
  ├── <meta property="og:*"> — Open Graph tags
  ├── <script defer src="alpinejs CDN"> — Alpine.js
  ├── (optional) additional <script> CDN tags (e.g. Chart.js)
  └── <style>
        ├── Reset (box-sizing, margin, padding)
        ├── :root { CSS custom properties }
        ├── body
        ├── .header, .header-badge
        ├── .card (main input card)
        ├── .field, label, input styles
        ├── .btn (action buttons)
        ├── .results, .result-card (output cards)
        ├── .disclaimer (footer text)
        ├── @keyframes slideUp + .animate-in + .delay-N
        └── @media (max-width: 420px) { responsive overrides }
      </style>
</head>
<body x-data="componentName()">
  ├── .header (badge + optional heading)
  ├── .card (inputs + action button)
  ├── <template x-if="hasResults"> results section </template>
  ├── .disclaimer
  └── <script>
        ├── (optional) helper functions (e.g. parseNaturalDate)
        └── function componentName() { return { ...Alpine component... } }
      </script>
</body>
</html>
```

---

## Conventions

### Naming
- **Directory names** are lowercase, kebab-case (e.g. `days-between`)
- **Alpine component functions** are camelCase (e.g. `dateDiff`, `loanCalc`, `taxCalc`)
- **CSS classes** are lowercase kebab-case (e.g. `result-card`, `header-badge`)
- **IDs on the home page** follow `tool-<directory-name>` (e.g. `tool-home-affordability`)

### Currency formatting (INR)
Most tools include helper functions for Indian number formatting:
- `formatNum(n)` — locale-formatted integer: `12,34,567`
- `formatINR(n)` — compact format: `12.35 L`, `1.5 Cr`
- `fmt(n)` — with ₹ prefix, used in home-affordability

### Date tools
`days-between` and `days-calculator` share the same natural language date parser (`parseNaturalDate`). If adding a new date-related tool, copy and reuse this parser.

### Animations
All tools use the same `slideUp` keyframe animation applied via `.animate-in` and staggered with `.delay-1` through `.delay-4` classes.

---

## Common Modification Scenarios

| Scenario | Files to edit |
|----------|---------------|
| Change the colour palette | `:root` variables in **every** `index.html` |
| Add a new tool | New directory + `index.html`, **plus** a card in root `index.html` |
| Edit an existing tool's logic | Only that tool's `index.html` |
| Change the home page layout | Root `index.html` only |
| Add a new shared CDN library | Only the tool(s) that need it |
