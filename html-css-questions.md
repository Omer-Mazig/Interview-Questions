# HTML and CSS Interview Questions

## HTML Fundamentals

### 1. What is the difference between `<div>` and `<span>` elements?

<details>
<summary>View Answer</summary>

`<div>` is a block-level element that starts on a new line and takes up the full width available. `<span>` is an inline element that only takes up as much width as necessary and doesn't force a new line. `<div>` is typically used for larger sections of content, while `<span>` is used for smaller portions of text within a line.

</details>

### 2. What is the purpose of the `alt` attribute in the `<img>` tag?

<details>
<summary>View Answer</summary>

The `alt` attribute provides alternative text for an image if it cannot be displayed. It's also essential for accessibility as screen readers use this text to describe the image to visually impaired users.

</details>

### 3. What is semantic HTML and why is it important?

<details>
<summary>View Answer</summary>

Semantic HTML involves using HTML elements that clearly describe their meaning to both the browser and the developer. Examples include `<header>`, `<footer>`, `<article>`, and `<section>`. It's important because it:

- Improves accessibility
- Enhances SEO
- Makes code more readable and maintainable
- Provides clear structure to the document
</details>

### 4. Explain the difference between `localStorage` and `sessionStorage`.

<details>
<summary>View Answer</summary>

Both are web storage options that allow storing key-value pairs in a web browser.

- `localStorage`: Data persists until explicitly deleted, even when the browser is closed and reopened.
- `sessionStorage`: Data is cleared when the page session ends (when the browser tab is closed).
</details>

### 5. What is the purpose of the `<!DOCTYPE html>` declaration?

<details>
<summary>View Answer</summary>

It informs the browser about the HTML version used in the document. `<!DOCTYPE html>` specifically declares the document as HTML5. Without it, browsers may enter "quirks mode," which may cause rendering inconsistencies.

</details>

## CSS Concepts

### 1. Explain the box model in CSS.

<details>
<summary>View Answer</summary>

The CSS box model describes the rectangular boxes generated for elements in the document tree. It consists of:

- Content: The actual content of the element
- Padding: Space between the content and the border
- Border: A line that surrounds the padding
- Margin: Space outside the border
</details>

### 2. What is the difference between `display: none` and `visibility: hidden`?

<details>
<summary>View Answer</summary>

- `display: none` removes the element from the document flow; it takes up no space.
- `visibility: hidden` hides the element but keeps its space in the layout.
</details>

### 3. Explain CSS specificity.

<details>
<summary>View Answer</summary>

Specificity is how browsers decide which CSS property values are the most relevant to an element. The specificity hierarchy (from lowest to highest):

1. Type selectors (e.g., h1) and pseudo-elements (e.g., ::before)
2. Class selectors, attribute selectors, and pseudo-classes
3. ID selectors
4. Inline styles
5. !important rule (overrides all of the above)
</details>

### 4. What are CSS preprocessors and what benefits do they provide?

<details>
<summary>View Answer</summary>

CSS preprocessors (like SASS, LESS, Stylus) are scripting languages that extend CSS and compile into regular CSS. Benefits include:

- Variables for reusable values
- Nesting to better visualize hierarchy
- Mixins for reusable code blocks
- Functions for computational logic
- Modularity with imports/partials
</details>

### 5. What are Flexbox and Grid, and when would you use one over the other?

<details>
<summary>View Answer</summary>

- **Flexbox** is a one-dimensional layout method for arranging items in rows or columns. It's ideal for:

  - Components of an application (navigation menus)
  - Small-scale layouts
  - Aligning items within a container

- **Grid** is a two-dimensional layout system for rows and columns. It's better for:
  - Larger scale layouts
  - Complex grid-based designs
  - Two-dimensional layouts where both row and column control is needed
  </details>

## Advanced Questions

### 1. What is ARIA and why is it important?

<details>
<summary>View Answer</summary>

ARIA (Accessible Rich Internet Applications) is a set of attributes that define ways to make web content and applications more accessible to people with disabilities. It's important because it helps bridge gaps in HTML semantics to create more accessible web applications, especially for dynamic content and advanced UI controls.

</details>

### 2. Explain the concepts of progressive enhancement and graceful degradation.

<details>
<summary>View Answer</summary>

- **Progressive enhancement** starts with a basic functional experience and adds enhanced functionality for modern browsers.
- **Graceful degradation** starts with a modern experience and ensures it remains functional (though possibly with fewer features) in older browsers.
</details>

### 3. What are CSS Custom Properties (CSS Variables) and what problems do they solve?

<details>
<summary>View Answer</summary>

CSS Custom Properties allow developers to define reusable values in CSS. They help solve problems like:

- The need to find and replace values across large stylesheets
- Making themes and style variations more manageable
- Creating responsive designs that adapt to different contexts
- Enabling runtime changes to styles without JavaScript manipulation of inline styles
</details>

### 4. What is Critical CSS and why is it important for performance?

<details>
<summary>View Answer</summary>

Critical CSS is the minimum CSS required to render the above-the-fold content of a webpage. It's important for performance because:

- It can be inlined in the HTML to eliminate render-blocking CSS
- It reduces the time to first meaningful paint
- It improves perceived load time for users
</details>
