# AMAS Questions – CSS, HTML and Python

## 1. What is `z-index` in CSS?

`z-index` is used to decide which element appears on top of another element.

```css
.box {
    z-index: 2;
}
```

A higher `z-index` usually means the element appears in front.

## 2. What does `repeat()` do in CSS?

`repeat()` is used in CSS Grid to repeat columns or rows without writing them one by one.

```css
grid-template-columns: repeat(3, 1fr);
```

This creates 3 equal columns.

## 3. Which tag is used to make a horizontal line?

The `<hr>` tag is used to create a horizontal line.

## 4. What is the difference between `int` and `float`?

- `int` is used for whole numbers, like `10` or `25`.
- `float` is used for decimal numbers, like `10.5` or `3.14`.

## 5. How can we make a gap between lines?

We can use the CSS `line-height` property to control the space between lines of text.

```css
p {
    line-height: 1.5;
}
```

## 6. What does `fr` mean in CSS Grid?

`fr` means fraction. It divides the available space into parts in a Grid layout.

```css
grid-template-columns: 1fr 2fr;
```

The second column gets twice the space of the first column.

## 7. Which tag is used to add an item in HTML?

The `<li>` tag is used to add a list item inside an ordered list (`<ol>`) or unordered list (`<ul>`).

## 8. Which tag is used to make an unordered list?

The `<ul>` tag is used to create an unordered list. Each item inside it uses the `<li>` tag.

```html
<ul>
    <li>Apple</li>
    <li>Orange</li>
</ul>
```

## 9. What is the use of a media query?

A media query is used to apply different CSS styles based on the screen size or device. It helps make a website responsive.

```css
@media (max-width: 600px) {
    body {
        font-size: 14px;
    }
}
```

## 10. How can we center an element in all directions using Flexbox?

We can use `justify-content` and `align-items` with `display: flex`.

```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

This centers the element horizontally and vertically.

## 11. What is the difference between content width and box width in `box-sizing`?

- `content-box`: The width applies only to the content. Padding and border are added to the total width.
- `border-box`: The width includes the content, padding and border.

```css
box-sizing: border-box;
```

## 12. How can we add validation to an input in HTML?

We can use HTML attributes like `required`, `type`, `min`, `max`, and `pattern` to validate input.

```html
<input type="email" required>
```

This makes the field required and checks that the entered value follows the email format.

## 13. What is the purpose of declaring `<!DOCTYPE html>`?

`<!DOCTYPE html>` tells the browser that the document uses HTML5. It helps the browser render the page in standards mode.

## 14. What is `minmax()` in CSS?

`minmax()` is used in CSS Grid to set a minimum and maximum size for a row or column.

```css
grid-template-columns: repeat(3, minmax(200px, 1fr));
```

Each column will be at least 200px and can grow to take available space.
