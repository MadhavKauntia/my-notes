# Dynamic Styling in React

## CSS Dynamic Styling

- To add dynamic styling to React components, we can use States.
- We define a state to store a flag or value based on which we will change the style.
- We can define custom class styling in the css files.
- Then, while defining className of the HTML component, we can use the following syntax:

```
<div className={`class1 ${stateCondition ? 'class2' : ''}`}>
```

In our css file, we can set separate styling in the following way.

```css
.class1 {
  color: black;
}

.class1.class2 {
  color: red;
}
```

## Styled Components

[Styled Components](www.styled-components.com)

> npm install --save styled-components

- Creating a button with styling using styled components looks like this.

```js
import styled from "styled-components";

const Button = styled.button`
  font: inherit;
  padding: 0.5rem 1.5rem;
  border: 1px solid #8b005d;
  color: white;
  background: #8b005d;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.26);
  cursor: pointer;

  &:focus {
    outline: none;
  }

  &:hover,
  &:active {
    background: #ac0e77;
    border-color: #ac0e77;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
  }
`;
```

- Dynamic styling can be created in styled components by passing props in this way.

```js
const FormControl = styled.div`
  margin: 0.5rem 0;

  & label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
    color: ${(props) => (props.invalid ? "red" : "black")};
  }

  & input {
    display: block;
    width: 100%;
    border: 1px solid ${(props) => (props.invalid ? "red" : "#ccc")};
    background: ${(props) => (props.invalid ? "#ffd7d7" : "transparent")};
    font: inherit;
    line-height: 1.5rem;
    padding: 0 0.25rem;
  }

  & input:focus {
    outline: none;
    background: #fad0ec;
    border-color: #8b005d;
  }
`;
```

```
<FormControl invalid={!isValid} />
```

## CSS Modules

- In this method, we rename our css files from `*.css` to `*.module.css` and import them using:
  > import styles from './\*.module.css';
- When declaring className for a component, we do it using `className={styles.['class-name']}`
