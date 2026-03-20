---
description: How to add a new calculator tool to Tiny Toolset
---

# Add a New Tool to Tiny Toolset

Follow these steps **in order** to add a new calculator tool that is fully consistent with the existing codebase.

## Prerequisites

Before starting, read these files for context:
- `.agent/ARCHITECTURE.md` — understand the project structure and conventions
- `.agent/DESIGN_SYSTEM.md` — reference for all tokens, components, and CSS patterns

---

## Step 1 — Create the tool directory

Create a directory with a lowercase kebab-case name that describes the tool:

```bash
mkdir <tool-directory-name>
```

Example: `percentage-calculator`, `bmi-calculator`, `sip-calculator`

---

## Step 2 — Create `index.html` using the template

Create `<tool-directory-name>/index.html` with this skeleton. Replace all `{{placeholders}}`:

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>{{Tool Name}} — {{Short Purpose Description}}</title>
  <meta name="description"
    content="{{1-2 sentence description of what this tool does.}}" />
  <meta property="og:title" content="{{Tool Name}}" />
  <meta property="og:description"
    content="{{Short OG description}}" />
  <meta property="og:type" content="website" />
  <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
  <!-- Add additional CDN scripts here if needed (e.g. Chart.js) -->
  <style>
    *,
    *::before,
    *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    :root {
      --bg: #f5f4f0;
      --surface: #ffffff;
      --surface-alt: #f9f8f5;
      --border: #e2dfd8;
      --primary: #1a3a5c;
      --primary-light: #2a5080;
      --accent: #e8613a;
      --accent-light: #f5845e;
      --text: #1c2b3a;
      --text-muted: #6b7a8a;
      --text-light: #9aa6b0;
      --success: #2d7a4f;
      --radius: 12px;
      --radius-lg: 20px;
      --shadow: 0 4px 24px rgba(26, 58, 92, 0.08);
      --shadow-lg: 0 8px 40px rgba(26, 58, 92, 0.14);
      --font-serif: 'Georgia', 'Times New Roman', serif;
      --font: system-ui, -apple-system, sans-serif;
    }

    body {
      font-family: var(--font);
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem 1rem 4rem;
    }

    /* Header */
    .header {
      text-align: center;
      margin-bottom: 2.5rem;
      max-width: 540px;
    }

    .header-badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      background: var(--primary);
      color: #fff;
      font-size: 0.72rem;
      font-weight: 600;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      padding: 5px 14px;
      border-radius: 100px;
      margin-bottom: 1rem;
    }

    .header h1 {
      font-family: var(--font-serif);
      font-size: clamp(1.8rem, 5vw, 2.6rem);
      font-weight: normal;
      color: var(--primary);
      line-height: 1.2;
      margin-bottom: 0.6rem;
    }

    .header p {
      color: var(--text-muted);
      font-size: 0.95rem;
      line-height: 1.6;
    }

    /* Card */
    .card {
      background: var(--surface);
      border-radius: var(--radius-lg);
      box-shadow: var(--shadow);
      border: 1px solid var(--border);
      width: 100%;
      max-width: 520px;
      padding: 2rem;
    }

    /* Input group */
    .field {
      margin-bottom: 1.75rem;
    }

    .field label {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.82rem;
      font-weight: 600;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      color: var(--text-muted);
      margin-bottom: 0.6rem;
    }

    .input-wrapper {
      position: relative;
      display: flex;
      align-items: center;
    }

    .input-prefix {
      position: absolute;
      left: 14px;
      color: var(--text-muted);
      font-weight: 600;
      font-size: 1rem;
      pointer-events: none;
    }

    input[type="number"] {
      width: 100%;
      padding: 0.8rem 1rem 0.8rem 2.2rem;
      font-size: 1.1rem;
      font-weight: 600;
      color: var(--text);
      background: var(--surface-alt);
      border: 2px solid var(--border);
      border-radius: var(--radius);
      outline: none;
      transition: border-color 0.2s, box-shadow 0.2s;
      -moz-appearance: textfield;
    }

    input[type="number"]::-webkit-outer-spin-button,
    input[type="number"]::-webkit-inner-spin-button {
      -webkit-appearance: none;
    }

    input[type="number"]:focus {
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(26, 58, 92, 0.1);
    }

    /* Button */
    .btn {
      width: 100%;
      padding: 0.9rem;
      background: var(--accent);
      color: #fff;
      border: none;
      border-radius: var(--radius);
      font-size: 0.95rem;
      font-weight: 700;
      letter-spacing: 0.04em;
      cursor: pointer;
      transition: background 0.2s, transform 0.1s, box-shadow 0.2s;
      box-shadow: 0 4px 16px rgba(232, 97, 58, 0.3);
      margin-top: 0.25rem;
    }

    .btn:hover {
      background: var(--accent-light);
      box-shadow: 0 6px 20px rgba(232, 97, 58, 0.4);
    }

    .btn:active {
      transform: scale(0.98);
    }

    /* Error */
    .error-msg {
      background: #fff3f0;
      border: 1px solid #ffd0c0;
      color: var(--accent);
      font-size: 0.84rem;
      padding: 0.7rem 1rem;
      border-radius: var(--radius);
      margin-top: 1rem;
    }

    /* Results */
    .results {
      width: 100%;
      max-width: 520px;
      margin-top: 1.5rem;
    }

    .results-title {
      font-size: 0.78rem;
      font-weight: 700;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: var(--text-muted);
      margin-bottom: 0.75rem;
      padding-left: 4px;
    }

    .result-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius-lg);
      padding: 1.4rem 1.6rem;
      margin-bottom: 0.85rem;
      box-shadow: var(--shadow);
      transition: transform 0.2s, box-shadow 0.2s;
      position: relative;
      overflow: hidden;
    }

    .result-card:hover {
      transform: translateY(-2px);
      box-shadow: var(--shadow-lg);
    }

    .result-card::before {
      content: '';
      position: absolute;
      left: 0;
      top: 0;
      bottom: 0;
      width: 4px;
      background: var(--accent);
      border-radius: 4px 0 0 4px;
    }

    .result-row {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      padding: 0.4rem 0;
      border-bottom: 1px dashed var(--border);
    }

    .result-row:last-child {
      border-bottom: none;
      padding-bottom: 0;
    }

    .result-label {
      font-size: 0.8rem;
      color: var(--text-muted);
      font-weight: 500;
    }

    .result-value {
      font-size: 1rem;
      font-weight: 700;
      color: var(--text);
    }

    .result-value.highlight {
      font-size: 1.25rem;
      color: var(--primary);
    }

    /* Disclaimer */
    .disclaimer {
      max-width: 520px;
      margin-top: 1.5rem;
      font-size: 0.75rem;
      color: var(--text-light);
      text-align: center;
      line-height: 1.6;
    }

    /* Animation */
    @keyframes slideUp {
      from {
        opacity: 0;
        transform: translateY(16px);
      }

      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .animate-in {
      animation: slideUp 0.35s ease both;
    }

    .delay-1 {
      animation-delay: 0.05s;
    }

    .delay-2 {
      animation-delay: 0.12s;
    }

    .delay-3 {
      animation-delay: 0.19s;
    }

    @media (max-width: 420px) {
      .card {
        padding: 1.5rem;
      }

      .result-card {
        padding: 1.1rem 1.2rem;
      }
    }
  </style>
</head>

<body x-data="{{componentName}}()">

  <div class="header animate-in">
    <div class="header-badge">
      <svg width="12" height="12" viewBox="0 0 24 24" fill="currentColor">
        <!-- Pick a Material-style 24×24 icon path that represents this tool -->
        <path d="..." />
      </svg>
      {{Tool Badge Name}}
    </div>
    <!-- Optional heading and subtitle: -->
    <!-- <h1>...</h1> -->
    <!-- <p>...</p> -->
  </div>

  <div class="card animate-in delay-1">
    <!-- Input fields here -->
    <div class="field">
      <label>{{Field Label}}</label>
      <div class="input-wrapper">
        <span class="input-prefix">{{prefix like ₹ or %}}</span>
        <input type="number" x-model.number="{{fieldName}}" placeholder="..." />
      </div>
    </div>

    <button class="btn" @click="calculate()">
      {{Button Label}} →
    </button>

    <template x-if="error">
      <div class="error-msg">⚠ <span x-text="error"></span></div>
    </template>
  </div>

  <!-- Results -->
  <template x-if="results">
    <div class="results animate-in delay-2">
      <div class="results-title">Results</div>
      <div class="result-card">
        <div class="result-row">
          <span class="result-label">{{Label}}</span>
          <span class="result-value highlight" x-text="{{computed}}"></span>
        </div>
        <!-- More result rows -->
      </div>
    </div>
  </template>

  <p class="disclaimer">
    * {{Disclaimer text}}
  </p>

  <script>
    function {{componentName}}() {
      return {
        // State
        {{fieldName}}: null,
        results: null,
        error: '',

        calculate() {
          this.error = '';
          this.results = null;

          // Validation
          if (!this.{{fieldName}} || this.{{fieldName}} <= 0) {
            this.error = 'Please enter a valid value.';
            return;
          }

          // Calculation logic
          this.results = { /* ... */ };
        },

        // Optional helpers
        fmt(n) {
          return n.toLocaleString('en-IN');
        }
      }
    }
  </script>

</body>

</html>
```

---

## Step 3 — Add entry in root `index.html`

Open the root `index.html` and add a new `<a>` card **inside** `<div class="tools-grid">`, after the last existing tool card.

### 3a. Add a new icon gradient CSS class

In the `<style>` section, add a new `.tool-icon.{{your-class}}` rule:
```css
.tool-icon.{{your-class}} {
  background: linear-gradient(135deg, {{color1}}, {{color2}});
}
```
Pick a gradient that is **distinct** from existing ones (orange, blue, green, navy already used).

Suggested unused palettes:
- **Purple:** `#7c3aed`, `#a78bfa`
- **Teal:** `#0d9488`, `#5eead4`
- **Pink:** `#db2777`, `#f472b6`
- **Amber:** `#d97706`, `#fbbf24`

### 3b. Add a new delay class if needed

If this is the 5th+ tool, add `.delay-5 { animation-delay: 0.33s; }` etc.

### 3c. Add the card HTML

```html
<a href="./{{tool-directory}}/index.html" class="tool-card animate-in delay-{{N}}" id="tool-{{directory-name}}">
  <div class="tool-icon {{your-class}}">
    <svg viewBox="0 0 24 24" fill="currentColor">
      <path d="{{24x24 Material icon path}}" />
    </svg>
  </div>
  <div class="tool-content">
    <div class="tool-name">{{Tool Display Name}}</div>
    <div class="tool-desc">{{1-line description}}</div>
  </div>
  <div class="tool-arrow">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
      stroke-linejoin="round">
      <polyline points="9 18 15 12 9 6" />
    </svg>
  </div>
</a>
```

---

## Step 4 — Update README.md

Add a row to the **Tools** table and update the **Project Structure** section in `README.md`.

---

## Step 5 — Test

1. Open the root `index.html` in a browser — verify the new card appears and links correctly.
2. Open the new tool's `index.html` directly — verify:
   - The header badge, card, and animation render correctly.
   - Inputs and calculation logic work.
   - Results appear correctly after calculation.
   - Error states display properly.
   - The page is responsive at ≤420px width.

---

## Checklist

- [ ] New directory created with lowercase kebab-case name
- [ ] `index.html` follows the template above with all CSS tokens copied
- [ ] Alpine.js component function is `camelCase`
- [ ] All meta tags present (`<title>`, `description`, `og:*`)
- [ ] Animations applied: `.animate-in`, `.delay-1`, `.delay-2`, etc.
- [ ] Responsive `@media (max-width: 420px)` rules included
- [ ] Disclaimer at the bottom
- [ ] Card added to root `index.html` with unique icon gradient
- [ ] `README.md` updated
- [ ] Tested in browser
