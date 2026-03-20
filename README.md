# 🧰 Tiny Toolset

A curated collection of simple, beautiful calculators for everyday math — home loans, date calculations, income tax, and more.

> **Live & lightweight.** Every tool runs entirely in the browser with zero build steps, no bundlers, and no backend required.

---

## 📦 Tools

| Tool | Description |
|------|-------------|
| **Home Affordability Calculator** | Find out how much home loan you can afford based on your monthly EMI budget. Compare 10, 20, and 30-year loan terms instantly. |
| **Days Between Calculator** | Calculate the exact number of days between any two dates using natural language input (e.g. *"today"*, *"next Friday"*, *"Jan 5"*). |
| **Days Calculator** | Add or subtract days from any date to find future or past dates effortlessly. |
| **Income Tax Calculator** | Estimate your income tax under India's New Tax Regime (FY 2025-26) with yearly & monthly breakdown, effective rate, and a visual doughnut chart. |

---

## 🛠 Technologies Used

| Technology | Purpose |
|------------|---------|
| **HTML5** | Semantic page structure and content |
| **CSS3** | Styling with CSS custom properties, responsive design, animations, and a consistent design system across all tools |
| **JavaScript** | Core calculator logic, natural language date parsing, and DOM interaction |
| **Alpine.js** | Lightweight reactive data-binding for interactive UI without a heavy framework |
| **Chart.js** | Doughnut chart visualisation in the Income Tax Calculator |

---

## 🏗 Project Structure

```
tinytoolset/
├── index.html                        # Home page — tool directory
├── home-affordability/
│   └── index.html                    # Home Loan / EMI Calculator
├── days-between/
│   └── index.html                    # Days Between two dates
├── days-calculator/
│   └── index.html                    # Add / Subtract days from a date
├── income-tax-calculator/
│   └── index.html                    # India New Regime Tax Calculator
└── README.md
```

---

## 🚀 Getting Started

No installation or build step required. Simply open any `index.html` file in your browser:

```bash
# Clone the repo
git clone https://github.com/<your-username>/tinytoolset.git

# Open the home page
open tinytoolset/index.html
```

Or serve it locally with any static server:

```bash
npx serve tinytoolset
```

---

## 🎨 Design

All tools share a unified, warm design language featuring:

- A **neutral warm palette** (`#f5f4f0` background, `#1a3a5c` primary, `#e8613a` accent)
- **Serif + system font** typography pairing for a polished editorial feel
- **Slide-up entrance animations** and hover micro-interactions
- **Responsive layouts** optimised for desktop and mobile
- **Card-based UI** with subtle shadows, accent borders, and badge highlights

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
