# Tiny Toolset — Design System Reference

> **Purpose:** Canonical reference for all design tokens, component patterns, and CSS conventions.  
> Any new tool or UI change **must** follow these specifications to stay consistent.

---

## 1. CSS Custom Properties (Design Tokens)

These must appear in the `:root` block of **every** tool's `<style>` tag.

```css
:root {
  /* Backgrounds */
  --bg: #f5f4f0;              /* Page background — warm off-white */
  --surface: #ffffff;          /* Card / panel background */
  --surface-alt: #f9f8f5;     /* Input backgrounds, alternating rows */

  /* Borders */
  --border: #e2dfd8;           /* Default border colour */

  /* Primary palette */
  --primary: #1a3a5c;         /* Deep navy — headings, badges, sliders */
  --primary-light: #2a5080;   /* Lighter navy — hover states */

  /* Accent palette */
  --accent: #e8613a;          /* Warm orange — CTAs, accents, errors */
  --accent-light: #f5845e;    /* Lighter orange — hover on accent */

  /* Text colours */
  --text: #1c2b3a;            /* Body text */
  --text-muted: #6b7a8a;      /* Labels, secondary text */
  --text-light: #9aa6b0;      /* Hints, disclaimers, placeholders */

  /* Semantic */
  --success: #2d7a4f;         /* Green — positive results */

  /* Layout */
  --radius: 12px;             /* Input fields, small cards */
  --radius-lg: 20px;          /* Main cards, result cards */

  /* Shadows */
  --shadow: 0 4px 24px rgba(26, 58, 92, 0.08);      /* Default card shadow */
  --shadow-lg: 0 8px 40px rgba(26, 58, 92, 0.14);    /* Hover / elevated shadow */

  /* Typography */
  --font-serif: 'Georgia', 'Times New Roman', serif;  /* Headings, big numbers */
  --font: system-ui, -apple-system, sans-serif;        /* Body text, labels */
}
```

### Additional tokens (income-tax-calculator only)
```css
--success-bg: #e8f5ee;     /* Green tinted card background */
--danger-bg: #fff3f0;      /* Red/orange tinted card background */
```

---

## 2. Typography

| Element | Font | Size | Weight | Colour |
|---------|------|------|--------|--------|
| Page heading (`h1`) | `--font-serif` | `clamp(1.8rem, 5vw, 2.6rem)` | normal | `--primary` |
| Header badge | `--font` | `0.72rem` | 600 | `#fff` on `--primary` bg |
| Section title | `--font` | `0.78rem` | 700, uppercase, `0.1em` tracking | `--text-muted` |
| Field label | `--font` | `0.82rem` | 600, uppercase, `0.06em` tracking | `--text-muted` |
| Input text | `--font` | `1rem–1.1rem` | 600 | `--text` |
| Result big number | `--font-serif` | `1.2rem–2.8rem` | normal | `--primary` |
| Result label | `--font` | `0.8rem` | 500 | `--text-muted` |
| Result value | `--font` | `1rem` | 700 | `--text` |
| Disclaimer | `--font` | `0.75rem` | normal | `--text-light` |

---

## 3. Component Patterns

### 3.1 Header (all tools)

Every tool starts with a `.header` section containing a `.header-badge` pill:

```html
<div class="header animate-in">
  <div class="header-badge">
    <svg width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
      <!-- 24×24 Material-style icon path -->
    </svg>
    Tool Name
  </div>
  <!-- Optional: <h1>, <p> below the badge -->
</div>
```

**Badge styling:** `background: var(--primary)`, white text, `border-radius: 100px`, uppercase, `0.72rem`.

### 3.2 Card (input container)

```html
<div class="card animate-in delay-1">
  <!-- .field elements go here -->
  <!-- Action button(s) -->
</div>
```

- Background: `var(--surface)`
- Border: `1px solid var(--border)`
- Radius: `var(--radius-lg)` (20px)
- Shadow: `var(--shadow)`
- Padding: `2rem` (desktop), `1.5rem` (mobile ≤420px)
- Max-width: `520px`

### 3.3 Field (input group)

```html
<div class="field">
  <label>FIELD LABEL</label>
  <!-- Input (text, number, or range) -->
</div>
```

- Bottom margin: `1.75rem` between fields
- Label: uppercase, muted, 0.82rem

### 3.4 Input styles

**Number inputs:**
```css
padding: 0.8rem 1rem 0.8rem 2.2rem;  /* extra left for ₹ prefix */
background: var(--surface-alt);
border: 2px solid var(--border);
border-radius: var(--radius);
```
Focus: `border-color: var(--primary)` + `box-shadow: 0 0 0 3px rgba(26,58,92,0.1)`

**Text inputs (date tools):**
Same as above but without the left padding adjustment. Use `.is-valid` (green border) and `.is-invalid` (accent/red border) classes for validation feedback.

**Range sliders:**
Custom styled with `--pct` CSS variable for fill. Thumb: white circle with `--primary` border.

### 3.5 Buttons

Primary action button:
```css
.btn {
  background: var(--accent);          /* #e8613a */
  color: #fff;
  border-radius: var(--radius);       /* 12px */
  font-weight: 700;
  box-shadow: 0 4px 16px rgba(232, 97, 58, 0.3);
}
```
Hover: `background: var(--accent-light)`, deeper shadow.  
Active: `transform: scale(0.98)`.

Secondary button (days-calculator):
```css
.btn-subtract {
  background: var(--surface-alt);
  color: var(--primary);
  border: 2px solid var(--border);
}
```

### 3.6 Results section

Wrapped in `<template x-if="hasResults">` so it only appears after calculation:

```html
<template x-if="results">
  <div class="results animate-in delay-2">
    <div class="results-title">RESULT HEADING</div>
    <div class="result-card">
      <!-- Result rows -->
    </div>
  </div>
</template>
```

#### Result card
- Background: `var(--surface)`, border, radius-lg, shadow
- Left accent bar (4px wide, absolute positioned `::before`)
- Hover: `translateY(-2px)` + `--shadow-lg`

#### Result row
```html
<div class="result-row">
  <span class="result-label">Label</span>
  <span class="result-value">Value</span>
</div>
```
Rows separated by `border-bottom: 1px dashed var(--border)`.

### 3.7 Disclaimer

```html
<p class="disclaimer">
  * Disclaimer text here.
</p>
```
Centered, `0.75rem`, `var(--text-light)`, max-width `520px`.

---

## 4. Entrance Animations

Every tool uses the same keyframe:

```css
@keyframes slideUp {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}

.animate-in { animation: slideUp 0.35s ease both; }
.delay-1 { animation-delay: 0.05s; }
.delay-2 { animation-delay: 0.12s; }
.delay-3 { animation-delay: 0.19s; }
.delay-4 { animation-delay: 0.26s; }
```

**Apply in order:** header → card → results (staggered).

---

## 5. Icon Gradient Palette (Home Page)

Each tool card on the home page has a unique icon gradient:

| Tool | CSS class | Gradient |
|------|-----------|----------|
| Home Affordability | `.tool-icon.home` | `linear-gradient(135deg, var(--accent), var(--accent-light))` — orange |
| Days Between | `.tool-icon.days-between` | `linear-gradient(135deg, #3a5cc2, #6b8bef)` — blue |
| Days Calculator | `.tool-icon.days-calc` | `linear-gradient(135deg, var(--success), #4aba72)` — green |
| Income Tax | `.tool-icon.tax` | `linear-gradient(135deg, var(--primary), var(--primary-light))` — navy |

When adding a new tool, choose a **distinct gradient** that isn't already used.

---

## 6. Responsive Breakpoints

A single breakpoint at `max-width: 420px`:
- Card padding reduces to `1.5rem`
- Result card padding reduces  
- Grids may collapse to single column (e.g. date-row, results-grid)
- Icon size may shrink (home page: 42px on mobile vs 48px)

---

## 7. SEO Checklist

Every tool page must include:
- `<title>` — descriptive, includes tool type (e.g. "Days Calculator — Add or Subtract Days")
- `<meta name="description">` — 1–2 sentence summary
- `<meta property="og:title">` — shorter OG title
- `<meta property="og:description">` — OG description
- `<meta property="og:type" content="website">`
- Semantic HTML (`<label for>`, proper heading hierarchy)
