## React.memo()

Whenever a state in a component changes, it is re-evaluated and rendered by React. During this process, all the components in this component are also called again. This can lead to a lot of unnecessary function calls, thus affecting performance.
When we use `export default React.memo(Component)`, we're instructing React not to call this component if a state in the parent component changes.

## useCallback()

Similarly, whenever a state in the function is changed, all the variables are declared again. This is not a problem for primitive datatypes but for reference datatypes like a function or a list, this will lead to creating a new reference every time a state in the component changes. A function can be declared by using the useCallback() method in the following way.

```js
const add = useCallback((x, y) => {
  return x + y;
}, []); // this list contains the values which on changing will trigger a new function call
```

## useMemo

What useCallback() is to functions, useMemo() is to data. We can use this for expensive operations like sorting to make sure they are not performed during every state change.

```js
const sortedList = useMemo(() => {
  return items.sort((a, b) => a - b);
}, [items]); // again, this list contains variable which on changing will lead to re-evaluating the data
```
