# CSS3 Architecture, Layout & Styling Cheatsheet

**CSS3 (Cascading Style Sheets)** governs layout geometry, visual presentation, responsive adaptation, and interactive states. Organizing CSS into predictable layers—**Tokens $\rightarrow$ Reset $\rightarrow$ Structural Layout $\rightarrow$ Styling Components $\rightarrow$ Responsive Queries**—keeps large stylesheets maintainable and bug-free.

---

## 1. The CSS Box Model & Sizing Mathematics

Every rendered HTML element is a rectangular box made of four concentric layers from the inside out: **Content** $\rightarrow$ **Padding** $\rightarrow$ **Border** $\rightarrow$ **Margin**.

```css
/* Universal Box-Sizing Reset (Essential for predictable layouts) */
*,
*::before,
*::after {
  box-sizing: border-box;
}

.box-demo {
  width: 320px;
  padding: 1.5rem;             /* Inner cushion; inherits element background */
  border: 2px solid #cbd5e1;   /* Visible perimeter around padding */
  margin: 2rem auto;           /* Outer clearance; vertical margins can collapse */
}
```

### `content-box` vs. `border-box`
* **`content-box` (Browser Default):** `width` applies only to the inner text/content. Adding `20px` padding and a `2px` border makes the box `364px` wide, often breaking grid columns.
* **`border-box` (Modern Standard):** Locks `width` and `height` to the outer border edge—padding and borders shrink inward so a `320px` box stays exactly `320px` wide.

---

## 2. Positioning Contexts: Relative vs. Absolute

The `position` property controls whether an element stays in normal document flow and defines the coordinate anchor for offset properties (`top`, `right`, `bottom`, `left`).

```css
/* 1. Parent establishes a new Positioning Context */
.card-anchor {
  position: relative;
}

/* 2. Child is removed from normal flow and anchored to the parent's corners */
.badge-corner {
  position: absolute;
  top: 0.75rem;
  right: 0.75rem;
  z-index: 10;
}
```

| Position Value | Flow Behavior | Coordinate Anchor |
| :--- | :--- | :--- |
| `static` | Default normal flow; offsets ignored. | None. |
| `relative` | Stays in normal flow and reserves original space; acts as anchor for `absolute` children. | Nudged relative to its own normal position. |
| `absolute` | Removed completely from normal flow (space collapses). | Nearest ancestor with `position` $\neq$ `static` (or viewport if none exists). |
| `fixed` | Removed from flow; stays pinned during scroll. | Browser viewport. |
| `sticky` | Scrolls normally until hitting threshold (e.g., `top: 0`), then locks in place. | Nearest scrolling ancestor. |

---

## 3. Cascade & CSS Specificity Resolution

When multiple rules target the same element, the browser resolves conflicts using a 4-tier specificity weight tuple $(a, b, c, d)$:

| Tier | Selector Category | Example | Tuple |
| :--- | :--- | :--- | :--- |
| **1 ($a$)** | Inline `style` attribute | `<div style="color: red;">` | `(1, 0, 0, 0)` |
| **2 ($b$)** | ID selectors | `#site-header` | `(0, 1, 0, 0)` |
| **3 ($c$)** | Classes, attributes, pseudo-classes | `.card`, `[required]`, `:hover` | `(0, 0, 1, 0)` |
| **4 ($d$)** | Element types & pseudo-elements | `article`, `p`, `::before` | `(0, 0, 0, 1)` |
| **Zero** | Universal selector & combinators | `*`, `>`, `+`, `~`, `:where()` | `(0, 0, 0, 0)` |

* **Tie-Breaker Rule:** If specificity tuples are identical, the rule written **last** in source order wins.
* **Best Practice:** Keep specificity flat at `(0, 0, 1, 0)` by styling with single classes. Avoid IDs in CSS and reserve `!important` strictly for accessibility overrides (like `prefers-reduced-motion`).

---

## 4. Scalable Class Architecture: Structural vs. Styling Classes

Separating macro layout rules (**structural classes**) from visual skinning (**styling classes**) lets you rearrange page geometry without rewriting component themes.

### Common CSS Structural Classes (Skeleton & Alignment)
```css
/* Centers page content and caps readable line width */
.container {
  width: 100%;
  max-width: 1120px;
  margin-inline: auto;
  padding-inline: 1.5rem;
}

/* 1D Flexbox Helpers */
.flex-row {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.75rem;
}

.flex-between {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

/* Vertical Rhythm Stack (Owl Selector) */
.stack > * + * {
  margin-top: 1.5rem;
}
```

### Common CSS Styling Classes (Surfaces, Typography & Interactive States)
```css
/* Elevated Card Surface */
.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 14px rgba(15, 23, 42, 0.06);
}

/* Typography & Badge Utilities */
.text-muted { color: #475569; }
.text-lead  { font-size: 1.125rem; max-width: 65ch; }

.badge {
  display: inline-block;
  padding: 0.25rem 0.65rem;
  font-size: 0.75rem;
  font-weight: 600;
  border-radius: 999px;
}

/* Interactive Button States */
.btn {
  display: inline-flex;
  align-items: center;
  padding: 0.65rem 1.25rem;
  font-weight: 600;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.2s ease, transform 0.15s ease;
}

.btn-primary {
  background: var(--color-primary);
  color: #ffffff;
}

.btn-primary:hover,
.btn-primary:focus-visible {
  background: var(--color-primary-hover);
}
```

---

## 5. Modern Layout Engines: Flexbox & CSS Grid

### Flexbox (1D Axis Alignment)
Ideal for navbars, button groups, and single-axis alignment.
* `justify-content`: Distributes items along the **main axis** (`flex-start`, `center`, `space-between`, `space-evenly`).
* `align-items`: Aligns items along the **cross axis** (`stretch`, `center`, `flex-start`, `baseline`).
* `flex: 1`: Allows flex children to grow equally to fill available space.

### CSS Grid (2D Matrix Architecture)
Ideal for multi-column card grids, dashboards, and page skeletons.

```css
/* Explicit 3-column grid using fractional units (fr) */
.grid-3 {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}

/* Intrinsic Responsive Grid (wraps automatically with zero media queries) */
.grid-auto {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(270px, 1fr));
  gap: 1.5rem;
}
```

---

## 6. CSS Custom Properties (Design Tokens)

Define colors, spacing, and typography once in `:root` so theme updates cascade across your entire application instantly.

```css
:root {
  --color-bg: #f8fafc;
  --color-surface: #ffffff;
  --color-text: #0f172a;
  --color-primary: #2563eb;
  --color-primary-hover: #1d4ed8;
  --color-border: #e2e8f0;
  --space-md: 1.5rem;
  --radius-md: 12px;
}
```

---

## 7. Responsive Design & Media Queries

Build **mobile-first** using fluid units (`rem`, `%`, `clamp()`) and progressively enhance layouts upward with `min-width` breakpoints.

```css
/* Fluid Typography: Scales smoothly between 1.75rem and 2.75rem */
.hero-title {
  font-size: clamp(1.75rem, 3vw + 1rem, 2.75rem);
}

/* Mobile Default: Single-column form */
.form-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1.5rem;
}

/* Tablet & Up (768px+) */
@media (min-width: 768px) {
  .form-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Desktop & Up (1024px+) */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
  }
}

/* Accessibility Preference Query: Respects user motion sensitivity */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    transition-duration: 0.01ms !important;
    animation-duration: 0.01ms !important;
  }
}
```