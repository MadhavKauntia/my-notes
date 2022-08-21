# [Website Accessibility, v2](https://learn-a11y.netlify.app/)

- There is no direct high-level way of detecting if a user is using some kind of assistive technology. One way to implement this can be to add HTML to your website which can be disabled using CSS. While this text will not be visible to a user on screen, it will be read out if the user is using some kind of assistive tech like a screen reader.

```css
.visuallyhidden {
  position: absolute;
  left: 0;
  top: -500px;
  width: 1px;
  height: 1px;
  overflow: hidden;
}
```

- You can do a lot in accessibility by just using the proper semantic elements in HTML for what you're trying to accomplish.

- Use `label` tag for form attributes. This will allow screen readers to recite the label when the fiels is focused.

- If you ever need to label an element which does not support the `label` tag, we can use `aria-label` instead.

- To allow keyboard only users to tab to an element, we can set the attribute `tabindex="0"` to that element.

- Keyboard shortcuts can be implemented using JS which can greatly enhance the user experience.

- _Skip links_ help users skip over large headers and navigation and jump into the "main" content of the page.
