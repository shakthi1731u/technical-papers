# CSS Cheatsheet: Styling & Layout Reference

**CSS (Cascading Style Sheets)** controls how your HTML looks on screen—colors, typography, spacing, animations, and responsive layouts. If HTML is the structure of the house, CSS is the interior design and architecture.

---

## 1. How to Add CSS to HTML

```html
<!-- 1. External Stylesheet (Best Practice: keeps code clean and reusable) -->
<link rel="stylesheet" href="styles.css">

<!-- 2. Internal Stylesheet (Good for single-page prototypes) -->
<style>
  p { color: navy; }
</style>

<!-- 3. Inline Style (Avoid for routine styling; overrides almost everything) -->
<p style="color: crimson;">Urgent alert</p>
```

---

## 2. Basic Syntax & Selectors

Every CSS rule consists of a **selector** (what you want to style) and a **declaration block** (how you want to style it).

```css
selector {
  property: value;
}
```

### Essential Selectors

| Selector | Example | What it Targets |
|---|---|---|
| Universal | `*` | Every single element on the page |
| Element | `p` | All `<p>` tags |
| Class | `.card` | Any element with `class="card"` (reusable) |
| ID | `#header` | The single element with `id="header"` |
| Descendant | `div p` | All `<p>` elements anywhere inside a `<div>` |
| Direct Child | `div > p` | Only `<p>` elements directly inside a `<div>` |
| Group | `h1, h2, h3` | All listed selectors simultaneously |
| Compound | `button.primary` | `<button>` elements that also have the class `primary` |
| Attribute | `input[type="email"]` | Inputs matching a specific attribute value |
| Pseudo-class | `a:hover` | Elements in a special state (hovered, focused, etc.) |
| Structural | `li:nth-child(2)` | The 2nd `<li>` inside its parent |
| Pseudo-element | `p::before` | Virtual elements injected before/after content |

---

## 3. Cascade, Specificity, and Inheritance

When multiple CSS rules target the same element, the browser uses a strict tie-breaking system:

1. **Specificity Hierarchy** (from strongest to weakest):
   * Inline styles (`style="..."`)
   * ID selectors (`#nav`)
   * Classes, attributes, and pseudo-classes (`.btn`, `:hover`)
   * Elements and pseudo-elements (`div`, `::before`)
2. **Source Order**: If two rules have identical specificity, the one written **last** wins.
3. **Inheritance**: Text properties (`color`, `font-family`, `line-height`) naturally trickle down from parent to child; box properties (`margin`, `padding`, `border`) do not.
4. **`!important`**: Forces a property to win regardless of specificity. Treat this as an emergency eject button—overusing it makes stylesheets hard to debug.

---

## 4. Colors and Backgrounds

```css
.hero-banner {
  color: #1e293b;                        /* Text color */
  background-color: #f8fafc;             /* Fallback background color */
  background-image: url("hero-bg.jpg");
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;                /* Scales image to fill container without stretching */
  opacity: 0.95;                         /* 0 (invisible) to 1 (solid) */
}
```
* **Color formats**: Named (`tomato`), Hex (`#2563eb`), RGB/RGBA (`rgba(37, 99, 235, 0.5)`), and HSL/HSLA (`hsl(221, 83%, 53%)` — great for tweaking brightness/saturation).

---

## 5. Typography & Text Styling

```css
body {
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  font-size: 16px;
  font-weight: 400;        /* 400 = normal, 700 = bold */
  line-height: 1.6;        /* Unitless multiplier; 1.5–1.6 is ideal for body readability */
}

h1 {
  text-align: center;      /* left, right, center, justify */
  text-transform: uppercase;
  letter-spacing: 0.05em;
  text-decoration: none;   /* Often used to remove underlines from <a> links */
}
```

---

## 6. The CSS Box Model

Every HTML element is secretly a rectangular box made of four layers from the inside out: **Content $\rightarrow$ Padding $\rightarrow$ Border $\rightarrow$ Margin**.

```css
.card {
  width: 300px;
  padding: 20px;               /* Cushioning inside the border */
  border: 2px solid #cbd5e1;   /* Visible edge around the padding */
  margin: 24px;                /* Invisible breathing room outside the border */
  box-sizing: border-box;      /* Makes width = 300px TOTAL (includes padding & border) */
}
```

### Margin & Padding Shorthand Clockwise Rule
Think of a clock face (**Top $\rightarrow$ Right $\rightarrow$ Bottom $\rightarrow$ Left**):
```css
margin: 10px;                 /* All 4 sides */
margin: 10px 20px;            /* Top & Bottom | Left & Right */
margin: 10px 20px 30px;       /* Top | Left & Right | Bottom */
margin: 10px 20px 30px 40px;  /* Top | Right | Bottom | Left */
```

---

## 7. Display & Positioning

### `display`
* `block`: Full width, starts on a new line.
* `inline`: Flows with text; ignores `width`, `height`, and top/bottom `margin`.
* `inline-block`: Flows inline like text, *but* respects `width`, `height`, and vertical margins.
* `flex` / `grid`: Activates modern 1D and 2D layout engines.
* `none`: Completely removes the element from the page (unlike `visibility: hidden`, which leaves an empty gap).

### `position`
```css
.badge {
  position: absolute; /* Positioned relative to the nearest parent with position: relative */
  top: -10px;
  right: -10px;
  z-index: 10;        /* Controls stacking order (higher numbers sit on top) */
}
```
* `static`: Default flow.
* `relative`: Nudges an element relative to where it *normally* sits, and acts as an anchor for `absolute` children.
* `fixed`: Locks to the browser window (great for floating chat buttons).
* `sticky`: Scrolls normally until it hits a threshold (like `top: 0`), then sticks in place (ideal for navbars).

---

## 8. Flexbox (1D Layouts)

Flexbox is built for arranging items in a **single row or column** and distributing space between them.

```css
.navbar {
  display: flex;
  flex-direction: row;            /* row | column | row-reverse | column-reverse */
  justify-content: space-between; /* Aligns along main axis: flex-start, center, space-between, space-evenly */
  align-items: center;            /* Aligns along cross axis: stretch, flex-start, center, flex-end */
  flex-wrap: wrap;                /* Allows items to wrap onto new lines on small screens */
  gap: 1rem;                      /* Clean spacing between items without messy margins */
}

.nav-item {
  flex: 1;                        /* Allows item to grow and share available space equally */
}
```

---

## 9. CSS Grid (2D Layouts)

CSS Grid shines when you need control over **both rows and columns** simultaneously.

```css
.gallery {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3 equal-width fractional columns */
  gap: 1.5rem;
}

/* Auto-responsive grid without media queries! */
.responsive-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}
```

---

## 10. Units of Measurement

| Unit | Type | Best Used For |
|---|---|---|
| `px` | Absolute | Crisp borders (`1px`), small shadows, fixed icon sizes |
| `%` | Relative | Fluid widths relative to the parent container |
| `rem` | Relative | **Font sizes, padding, and margins** (scales relative to root `<html>` font size, usually `16px`) |
| `em` | Relative | Spacing that should scale proportionally with the current element's font size |
| `vw` / `vh` | Viewport | Full-screen hero sections (`min-height: 100vh`) |
| `fr` | Grid | Dividing remaining space in CSS Grid tracks |

---

## 11. Visual Polish, States & Animations

```css
.btn {
  background-color: #2563eb;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 0.75rem 1.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.2s ease, background-color 0.2s ease;
}

.btn:hover {
  background-color: #1d4ed8;
  transform: translateY(-2px); /* Subtle lift on hover */
}

.btn:active {
  transform: scale(0.98);      /* Tactile press effect */
}

input:focus {
  outline: 2px solid #2563eb;  /* Crucial for keyboard accessibility */
  outline-offset: 2px;
}
```

---

## 12. CSS Variables & Responsive Media Queries

### Custom Properties (Variables)
Define your color palette and spacing tokens once in `:root` so you can update your whole site's theme in seconds:

```css
:root {
  --brand-color: #2563eb;
  --surface-bg: #ffffff;
  --radius-md: 0.5rem;
}

.card {
  background: var(--surface-bg);
  border-radius: var(--radius-md);
}
```

### Mobile-First Media Queries
Write base styles for mobile phones first, then layer on adjustments as the screen gets wider:

```css
/* Mobile default */
.container {
  width: 100%;
  padding-inline: 1rem;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    max-width: 720px;
    margin-inline: auto;
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .container {
    max-width: 1140px;
  }
}
```

---

## 13. Essential Starter CSS Reset

Drop this at the top of every new stylesheet to eliminate inconsistent browser default margins and make sizing predictable:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: system-ui, -apple-system, sans-serif;
  line-height: 1.5;
  -webkit-font-smoothing: antialiased;
}

img, picture, video, svg {
  display: block;
  max-width: 100%;
}
```