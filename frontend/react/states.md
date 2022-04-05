## useState

- Whenever we update state in a component, that component is reloaded.
- Suppose we want to update state that depends on previous state, we shouldn't simply use `setState({...prevState, title: 'Madhav})`. Instead, we should use the following format.

```js
setState((prevState) => {
  return { ...prevState, title: "Madhav" };
});
```

- In React, we can only communicate from parent to child and vice versa, we cannot communicate between siblings. We can store the state in a component which has access to both components to transfer data between two components.

![Lifting State Up](../images/lifting-state-up.png)

### Stateless vs Stateful Components

- **Stateless Component** - Also known as a _dumb_ component, it does not store any states.
- **Stateful Components** - These are the _smart_ components which store states.

### Controlled vs Uncontrolled COmponents

- **Controlled Component** - The values and changes in values are not handled in this component but in the parent component.
- **Uncontrolled Component** - The values and changes in values are handled in these components. These components basically control the _controlled components_.

* We need to specify a key attribute for rendering lists. If we don't specify keys, react re-renders all values of a list if any new item is added which causes a performance issue.
