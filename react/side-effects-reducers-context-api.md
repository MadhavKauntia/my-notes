## Side Effects

- We can store key value pairs in the local storage of the browser by using `localStore.setItem(key, value)` function. `localStorage` is a global variable available by default.
- We can return a function in the useEffect hook. This function is the cleanup function. It runs before every time useEffect is triggered except for the first time useEffect is triggered.

## Reducers

- useReducer() is used for State Management.
- Sometimes, we have _more complex state_ - for example if it has multiple states, multiple ways of changing it or dependencies to other states.

> const [state, dispatchFn] = useReducer(reducerFn, initialState, initFn);
