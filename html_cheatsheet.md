# HTML Cheatsheet: The Ultimate Beginner-to-Pro Guide

**HTML (HyperText Markup Language)** is the standard skeleton of every webpage. Think of it as the framing and drywall of a house: it defines what everything *is* (a heading, a paragraph, a button, an image) before CSS comes in to paint the walls and arrange the furniture.

---

## 1. Basic HTML Document Boilerplate

Every HTML page starts with a standard structure. In most code editors (like VS Code), typing `!` and pressing `Tab` generates this automatically.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="A brief summary of your page for search engines.">
  <title>Page Title</title>
</head>
<body>
  <h1>Hello World</h1>
  <p>Welcome to my first webpage.</p>
</body>
</html>
```

### What's happening here?
* **`<!DOCTYPE html>`**: Tells the browser to render the page using modern HTML5 standards.
* **`<html lang="en">`**: The root container. The `lang` attribute helps screen readers pronounce words correctly and aids search engines.
* **`<head>`**: The "brain" of the page. Nothing inside `<head>` is visible on the page itself; it holds metadata, page titles, and links to CSS stylesheets.
* **`<meta name="viewport" ...>`**: Crucial for mobile responsiveness—it ensures your site scales properly on phones and tablets.
* **`<body>`**: Everything visible to the user lives inside here.

---

## 2. Common Text & Structural Elements

| Tag | Purpose | Human Context / Tip |
|---|---|---|
| `<h1>`–`<h6>` | Headings | `<h1>` is the main title (use only one per page for good SEO); `<h6>` is the smallest subheading. |
| `<p>` | Paragraph | Automatically adds a bit of breathing room (margin) above and below the text. |
| `<br>` | Line break | Forces a single line break (great for poems or addresses, not for spacing out paragraphs). |
| `<hr>` | Thematic break | Renders a horizontal line to separate distinct topics or sections. |
| `<strong>` | Important text | Renders as **bold** and tells screen readers to emphasize the word strongly. |
| `<em>` | Emphasized text | Renders as *italic* and adds verbal stress for accessibility tools. |
| `<b>` / `<i>` | Bold / Italic | Purely visual styling without adding extra semantic importance. |
| `<span>` | Inline container | Wraps a small piece of text inside a line so you can style it with CSS. |
| `<div>` | Block container | A generic invisible box used to group elements together for layout styling. |
| `<pre>` | Preformatted text | Preserves exact spaces and line breaks from your code editor. |
| `<code>` | Code snippet | Displays text in a monospace font to represent computer code. |

---

## 3. Links and Images

Links connect the web together, and images bring pages to life.

```html
<!-- Standard Link -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">Open external link</a>

<!-- Jump to a section on the same page -->
<a href="#contact-section">Jump to Contact</a>

<!-- Image -->
<img src="images/photo.jpg" alt="A golden retriever playing fetch in a sunny park" width="400">
```

* **`href`**: The destination URL or internal file path.
* **`target="_blank"`**: Forces the link to open in a new browser tab.
* **`rel="noopener noreferrer"`**: A security and privacy best practice whenever using `target="_blank"`.
* **`src`**: The path or URL to the image file.
* **`alt`**: **Never skip this!** It describes the image for visually impaired users using screen readers and displays if the image fails to load.

---

## 4. Lists

HTML gives you three distinct ways to organize items:

```html
<!-- Unordered list (bullet points - order doesn't matter) -->
<ul>
  <li>Milk</li>
  <li>Bread</li>
</ul>

<!-- Ordered list (numbered - sequence matters) -->
<ol>
  <li>Preheat the oven to 180°C</li>
  <li>Bake for 25 minutes</li>
</ol>

<!-- Description list (terms and their definitions) -->
<dl>
  <dt>HTML</dt>
  <dd>The standard markup language for creating webpages.</dd>
</dl>
```

---

## 5. Tables

Use tables for **tabular data** (like spreadsheets, schedules, or pricing comparisons)—never for overall page layout.

```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Role</th>
      <th>City</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Sam</td>
      <td>Frontend Developer</td>
      <td>Bengaluru</td>
    </tr>
    <tr>
      <td>Priya</td>
      <td>UX Designer</td>
      <td>Chennai</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="3">Total Team Members: 2</td>
    </tr>
  </tfoot>
</table>
```

* `<table>`: Wraps the entire table.
* `<thead>`, `<tbody>`, `<tfoot>`: Logically groups the header, main rows, and summary footer.
* `<tr>`: Table Row.
* `<th>`: Table Header cell (bold and centered by default).
* `<td>`: Table Data cell.

---

## 6. Forms & User Input

Forms are how users interact with your site—logging in, searching, or sending messages.

```html
<form action="/submit" method="post">
  <!-- Always connect labels to inputs using 'for' and 'id' -->
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" placeholder="e.g. code_ninja" required>

  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required>

  <label for="city">City:</label>
  <select id="city" name="city">
    <option value="bengaluru">Bengaluru</option>
    <option value="chennai">Chennai</option>
  </select>

  <label for="message">Message:</label>
  <textarea id="message" name="message" rows="4" placeholder="Write something..."></textarea>

  <button type="submit">Send Message</button>
</form>
```

### Handy `<input>` Types
Using the right input type triggers helpful mobile keyboards (like a number pad for `tel`) and built-in browser validation:

* `type="text"` – Single-line text
* `type="password"` – Masks characters with dots
* `type="email"` – Validates for an `@` symbol
* `type="number"` – Numeric input with stepper arrows
* `type="date"` – Native calendar date picker
* `type="checkbox"` – Select zero, one, or many options
* `type="radio"` – Select only *one* option from a group (share the same `name` attribute)
* `type="file"` – File upload picker
* `type="search"` / `type="tel"` / `type="url"` – Specialized text inputs

---

## 7. Semantic HTML5 Elements

Instead of wrapping everything in meaningless `<div>` tags ("div soup"), use **semantic tags** that describe the role of the content. This drastically improves SEO and accessibility.

```html
<header>Top banner, logo, and intro</header>
<nav>Main navigation menu links</nav>
<main>
  <section>
    <h2>Latest Articles</h2>
    <article>An independent, self-contained blog post</article>
  </section>
  <aside>Related links, sidebar, or author bio</aside>
</main>
<footer>Copyright info, contact links, and sitemap</footer>
```

---

## 8. Audio and Video

Modern HTML makes embedding media straightforward without third-party plugins:

```html
<audio controls src="podcast-episode.mp3">
  Your browser does not support the audio element.
</audio>

<video controls width="640" poster="thumbnail.jpg">
  <source src="tutorial.mp4" type="video/mp4">
  Your browser does not support HTML video.
</video>
```
*(Tip: Adding `controls` gives the user play, pause, and volume buttons; `poster` sets a preview image before the video plays.)*

---

## 9. Essential Attributes

Attributes live inside the opening tag and modify an element's behavior or identity:

```html
<p id="welcome-banner" class="highlight text-large" title="Hover tooltip">Hello!</p>
```

* **`id`**: A **unique** identifier (can only be used once per page).
* **`class`**: A reusable label you can apply to many elements for CSS styling or JavaScript targeting.
* **`data-*`**: Custom attributes (e.g., `data-user-id="42"`) used to store extra info for JavaScript.
* **`disabled`**: Greys out a form input or button so it can't be clicked.
* **`readonly`**: Prevents editing an input's text, though users can still copy it.
* **`required`**: Prevents form submission until the field is filled out.
* **`hidden`**: Completely hides the element from view.

---

## 10. Block vs. Inline Elements

Understanding how elements sit on the page prevents 90% of early layout headaches:

* **Block-level elements** (`<div>`, `<p>`, `<h1>`–`<h6>`, `<section>`, `<ul>`): Always start on a new line and stretch out to take up the full width available.
* **Inline elements** (`<span>`, `<a>`, `<strong>`, `<em>`, `<img>`): Sit neatly alongside text without forcing a new line, only taking up as much width as their content needs.

---

## 11. Common HTML Entities

Some characters are reserved by HTML syntax (like `<` and `>`). Use entities to display them safely as text:

| Entity | Displays | Description |
|---|---|---|
| `&lt;` | `<` | Less-than sign |
| `&gt;` | `>` | Greater-than sign |
| `&amp;` | `&` | Ampersand |
| `&quot;` | `"` | Double quotation mark |
| `&copy;` | `©` | Copyright symbol |
| `&nbsp;` | *(space)* | Non-breaking space (prevents awkward line wraps between two words) |

---

## 12. Comments & Best Practices

```html
<!-- Comments are invisible in the browser; use them to leave notes for yourself or your team -->
```

### Golden Rules of HTML
1. **Separation of concerns**: Use HTML strictly for structure and meaning—leave colors, fonts, and layouts to CSS.
2. **Stay consistent**: Write all tags and attributes in lowercase and wrap attribute values in double quotes (`class="btn"`).
3. **Accessibility first**: Always pair `<label>` with `<input>`, include `alt` attributes on images, and follow logical heading hierarchy (`<h1>` $\rightarrow$ `<h2>` $\rightarrow$ `<h3>`).