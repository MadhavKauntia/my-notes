# [CSS Grid & Flexbox for Responsive Layouts, v2](https://frontendmasters.github.io/grid-flexbox-v2/)

## Flexbox

```css
.parent {
  display: flex;
  flex-direction: row; /* column */
  flex-wrap: wrap; /* nowrap, wrap-reverse */

  /* Lines 2 and 3 can be replaced with */
  flex-flow: row wrap; /* preferred for responsive layouts */

  justify-content: space-between; /* flex-start, flex-end, center, space-around, space-evenly */
  align-items: center; /* flex-start, flex-end, baseline, stretch */
  align-content: center; /* flex-start, flex-end, space-betweem, space-around, space-evenly */
}

.child {
  order: 2; /* sets order of an element */
  flex-basis: 25%; /* the property `width` should never be used inside of flexbox. Instead, use `flex-basis` */
}
```

## Header

- Use `<header>` tag for headers
- Display `<nav>` links using an unordered list `<ul>`
- Always set the nav links `display: block`.
- To select a particular element in a `<li>`, use `li:first-child` or `li:nth-child(x)`.
