# CSS Without the Chaos: The Case for Reusable Styles

Cascading Style Sheets (CSS) is one of several stylesheet languages used to control the presentation of HTML elements. A key part of writing CSS effectively is ensuring that style rules behave predictably. However, due to CSS's cascading nature, conflicts can arise when multiple styles are applied to the same element.


## Understanding the Cascade

The Cascade is a set of rules that determines which style declarations take precedence when multiple conflicting styles apply to an element. CSS evaluates these conflicts based on several factors, leading to a cascaded value-the final applied style.

```css
/* Global rule */
p {
    color: black;
}

/* More specific rule */

#special {
    color: red;
}
```

If an element has `<p>`, and `#special` tags, the ID selector wins because it has the highest specificity, and the text appears red.


## How the Cascade Resolves Conflicts

The cascade considers the following criteria to resolve conflicts:

1. Stylesheet Origin: CSS is applied from different sources:
    - Author styles (written by developers)
    - User styles (customized by users)
    - User agent styles (browser defaults)

Note: `!important` declarations override normal specificity rules.

```css
p {
    color: blue !important;
}
```

Specifity rules based on stylesheet origin are now in this order:
- Important user agent styles.
- Important user styles.
- Important author styles.
- Normal author styles.
- Normal user styles.
- Normal user agent styles.

2. Inline Styles: Applied directly to the element in HTML document file via the attribute, `style=""`, taking precedence over external styles.

3. Selector Specificity: The browser then considers the weight of importance of the selectors. A more specific selector will override a less specific one.

``` css
button {
    color: blue;
}

.button-class {
    color: green;
}

#button-id {
    color: red;
}
```

5. Source Order: If all else is equal, the browser then determines the last declared rule as the cascaded rule.

```css
p {
    color: black;
}

p {
    color: blue;
} /* Blue wins */
```

## Why Highly Specific CSS is Problematic

Writing highly specific styles using IDs, deeply nested selectors, or inline styles can lead to unpredictable problems and unnecessarily increased file size due to duplication of styles, and a lack of reusability.

### Lack of Reusability

```css
/* Highly specific styles */
#dashboard .profile-card h2 {
    font-size: 24px;
    color: blue;
}

#dashboard .profile-card p {
    font-size: 16px;
    color: gray;
}

```
#### Issues: 
- These styles only work in `#dashboard`.
- If you want to reuse these styles elsewhere, you'd have to rewrite them.
- If the HTML structure changes, these styles might break.


### Solution: Reusable CSS
This approach uses utiility classes that can be applied anywhere.

```css

/* Reusable styles */
.text-lg {
  font-size: 24px;
  font-weight: bold;
}

.text-sm {
  font-size: 16px;
}

.text-primary {
  color: blue;
}

.text-secondary {
  color: gray;
}

```

```html
<div class="profile-card">
  <h2 class="text-lg text-primary">John Doe</h2>
  <p class="text-sm text-secondary">Web Developer</p>
</div>

<div class="post-card">
  <h2 class="text-lg text-primary">New Blog Post</h2>
  <p class="text-sm text-secondary">Published today</p>
</div>
```


### File Size Comparison

```css

#dashboard .profile-card h2 { 
    font-size: 24px; 
    font-weight: bold; 
    color: blue; 
}

#dashboard .profile-card p { 
    font-size: 16px; 
    color: gray; 
}

#posts .post-card h2 { 
    font-size: 24px; 
    font-weight: bold; 
    color: blue; 
}

#posts .post-card p { 
    font-size: 16px; 
    color: gray; 
}

#notifications .notif-card h2 { 
    font-size: 24px; 
    font-weight: bold; 
    color: blue; 
}

#notifications .notif-card p { 
    font-size: 16px; 
    color: gray; 
}
```

Now compare it to a reusable approach:

```css 
.text-lg { 
    font-size: 24px; 
    font-weight: bold; 
}

.text-sm { 
    font-size: 16px; 
}

.text-primary { 
    color: blue; 
}

.text-secondary { 
    color: gray; 
}
```


## Maintainability and Scalabilty

Reusable CSS is maintainable, scalable and predictable and is the founding principle of most modern CSS libraries. Libraries like TailwindCSS use a utility-first reusable CSS framework while other libraries like Bootstrap use a component-based framework and Bulma is a mix of both frameworks, utility-based and component-friendly.


## Hidden CSS Gems

Some lesser-known helpful CSS features that promote maintainability include:

- `:where()`: takes a selector list and selects any elements that can be selected by one of the selectors in that list.
```css
:where(.text-xs, .text-lg) {
    color: blue;
}
```

- `:has()`: checks if any of the relative selectors that are passed as an argument match at least one element when anchored against this element.
```css
/* Selects an h1 element with a paragraph element that immediately follows the h1 and applies the style to the h1 */

h1:has(+ p) {
    margin-bottom: 0;
}
```

- `isolation` determines whether an element must create a new [stacking context](https://www.developer.mozilla.org/en-US/docs/Glossary/Stacking_context)

```css
/* Keyword values */
isolation: auto;
isolation: isolate;

/* Global values */
isolation: inherit;
isolation: initial;
isolation: revert;
isolation: revert-layer;
isolation: unset;
```

- `::marker` is a CSS pseudo-element which selects the marker box of a list-item.

```css 
li::marker {
    content: '* ';
    font-size: 1.2rem;
}
```

- `mix-blend-mode` is a CSS property which sets how an element's content should blend with the content of the element's parent and the element's background.

```css
mix-blend-mode: normal;

mix-blend-mode: multiply;

mix-blend-mode: hard-light;

mix-blend-mode: difference;

```

- `clamp()` clamps a middle value within a range of values between a defined minimum bound and a maximum bound. Dynamic sizing without media queries.

```css
font-size: clamp(16px, 2vw, 24px);
```

## Conclusion

Understanding the cascade  helps developers write predictable CSS. However, to build scalable and maintainable styles, reusability should be a priority. Modern CSS frameworks, like Tailwind and Bootstrap, encourage reusable styles, improving efficiency and consistency in development. By leveraging utility classes, modular styles, and new CSS features, we can write better CSS that is easier to maintain, scale, and reuse.
