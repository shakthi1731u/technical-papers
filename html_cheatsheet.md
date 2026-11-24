# HTML5 Architecture & Document Engineering Cheatsheet

**HTML5 (HyperText Markup Language)** defines the semantic document tree, accessibility roles, and machine-readable metadata of a webpage. Keeping HTML strictly focused on structure and meaning—while delegating all visual layout to CSS—creates resilient, accessible, and SEO-friendly applications.

---

## 1. Basic HTML5 Document Boilerplate & Common Header Meta Tags

The `<head>` element configures how browsers decode, scale, and index your page before rendering anything inside `<body>`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- 1. Character Encoding & Mobile Viewport -->
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- 2. SEO & Crawler Directives -->
  <meta name="description" content="A concise 150-character summary for search engine results.">
  <meta name="robots" content="index, follow">
  <link rel="canonical" href="https://example.com/current-page">

  <!-- 3. Browser UI & Social Preview (Open Graph) Tags -->
  <meta name="theme-color" content="#0f172a">
  <meta property="og:title" content="Web Architecture & CSS Layout Systems">
  <meta property="og:description" content="Technical reference for semantic HTML5 and modular CSS3.">
  <meta property="og:type" content="article">
  <meta property="og:image" content="https://example.com/preview-card.jpg">

  <title>Page Title | Brand</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h1>Hello World</h1>
  <p>Semantic document content.</p>
</body>
</html>
```

### Key Header Meta Tags Explained
* **`<meta charset="UTF-8">`**: Ensures universal character rendering across languages and symbols.
* **`<meta name="viewport" content="width=device-width, initial-scale=1.0">`**: Prevents mobile browsers from shrinking a desktop-sized canvas onto a phone screen.
* **`<meta name="description" ...>`**: Supplies the descriptive snippet displayed beneath your link in search engine results.
* **`<meta name="robots" ...>`**: Instructs search crawlers whether to index the page (`index`/`noindex`) and follow links (`follow`/`nofollow`).
* **`<meta property="og:*">`**: Open Graph tags used by social platforms and messaging apps to generate rich preview cards.
* **`<meta name="theme-color" ...>`**: Tints the mobile browser's top address bar to match your brand palette.

---

## 2. Document Flow: Inline vs. Block Elements

In normal HTML document flow, every rendered element defaults to either a **block-level** or **inline-level** formatting context.

| Feature | Block-Level Elements | Inline-Level Elements |
| :--- | :--- | :--- |
| **Line Behavior** | Always starts on a new line and pushes following elements down. | Flows horizontally within the current line of text. |
| **Default Width** | Expands to fill `100%` of its parent container's width. | Shrinks to fit only the width of its inner content. |
| **Dimensions (`width` / `height`)** | Fully respected. | Ignored (except *inline-replaced* elements like `<img>`). |
| **Vertical Margins (`top` / `bottom`)** | Fully respected (subject to vertical margin collapsing). | Ignored; only horizontal (`left` / `right`) margins apply. |
| **Common Examples** | `<div>`, `<p>`, `<h1>`–`<h6>`, `<section>`, `<article>`, `<header>`, `<nav>`, `<footer>`, `<ul>`, `<ol>`, `<li>`, `<table>`, `<form>` | `<span>`, `<a>`, `<strong>`, `<em>`, `<b>`, `<i>`, `<code>`, `<label>`, `<input>`, `<img>` |

```html
<!-- Block Example: Stacks vertically -->
<section class="card">
  <h2>Block Heading</h2>
  <p>This paragraph occupies the full width of its container.</p>
</section>

<!-- Inline Example: Flows smoothly inside a sentence -->
<p>
  Use <code>&lt;span&gt;</code> or <a href="#docs">anchor links</a> without breaking the line flow.
</p>
```

---

## 3. Semantic HTML5 Landmarks & Accessibility (a11y)

Replace generic `<div>` wrappers with **semantic landmarks** so screen readers, keyboard navigators, and search engines understand the hierarchy of your page.

```html
<header>Site logo, top banner, and primary controls</header>
<nav aria-label="Primary Navigation">Main navigation links</nav>

<main>
  <section aria-labelledby="pillars-heading">
    <h2 id="pillars-heading">Core Architectural Pillars</h2>
    <article>Self-contained, reusable content card or post</article>
  </section>
  <aside>Supplementary sidebar notes or related links</aside>
</main>

<footer>Copyright, legal links, and secondary navigation</footer>
```

### Accessibility Golden Rules
1. **Heading Hierarchy**: Use a single `<h1>` per page and step down logically (`<h1>` $\rightarrow$ `<h2>` $\rightarrow$ `<h3>`) without skipping levels.
2. **Explicit Form Labels**: Always link `<label for="id">` to `<input id="id">` so clicking the label focuses the input and screen readers announce its purpose.
3. **Meaningful `alt` Text**: Describe the informational content of `<img>` tags (`alt="Diagram of the CSS Box Model"`), or use `alt=""` for purely decorative graphics.

---

## 4. Common Text, Links, Lists & Media Elements

### Text & Phrasing
* `<strong>` / `<em>`: Semantic **bold** and *italic* emphasis (conveys vocal stress to assistive tech).
* `<code>` / `<pre>`: Inline code snippets and preformatted multi-line code blocks.
* `<hr>` / `<br>`: Thematic section divider and single line break.

### Links & Images
```html
<!-- External link with security attributes -->
<a href="https://developer.mozilla.org" target="_blank" rel="noopener noreferrer">MDN Web Docs</a>

<!-- Responsive image with descriptive alt text -->
<img src="box-model.png" alt="Concentric layers of content, padding, border, and margin" width="600" height="400">
```

### Lists & Tables
```html
<!-- Unordered, Ordered, and Description Lists -->
<ul><li>Bullet item</li></ul>
<ol><li>Numbered step</li></ol>
<dl>
  <dt>Specificity</dt>
  <dd>The algorithm browsers use to resolve conflicting CSS rules.</dd>
</dl>

<!-- Semantic Data Table -->
<table>
  <thead>
    <tr><th>Tier</th><th>Selector Type</th><th>Weight</th></tr>
  </thead>
  <tbody>
    <tr><td>1</td><td>Inline Style</td><td>(1,0,0,0)</td></tr>
    <tr><td>2</td><td>ID Selector</td><td>(0,1,0,0)</td></tr>
  </tbody>
</table>
```

---

## 5. Accessible Forms & Specialized Inputs

```html
<form action="/subscribe" method="post">
  <label for="full-name">Full Name:</label>
  <input type="text" id="full-name" name="full_name" placeholder="Ada Lovelace" required>

  <label for="email">Work Email:</label>
  <input type="email" id="email" name="email" placeholder="ada@example.org" required>

  <label for="topic">Primary Focus:</label>
  <select id="topic" name="topic">
    <option value="layout">Flexbox &amp; Grid</option>
    <option value="a11y">Semantic HTML &amp; Accessibility</option>
  </select>

  <button type="submit">Register</button>
</form>
```

---

## 6. Attributes & HTML Entities

* **Structural vs. Styling Hooks**: Reserve `id` for unique anchors (`#top`) and form associations (`for="email"`); use `class` for reusable CSS structural and styling rules.
* **Custom Data Attributes**: `data-state="open"` stores metadata cleanly for JavaScript interactions.
* **Reserved Entities**: `&lt;` (`<`), `&gt;` (`>`), `&amp;` (`&`), `&quot;` (`"`), `&copy;` (`©`), `&nbsp;` (non-breaking space).