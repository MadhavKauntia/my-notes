## Side Effects

- We can store key value pairs in the local storage of the browser by using `localStore.setItem(key, value)` function. `localStorage` is a global variable available by default.
- We can return a function in the useEffect hook. This function is the cleanup function. It runs before every time useEffect is triggered except for the first time useEffect is triggered.

## Reducers

- useReducer() is used for State Management.
- Sometimes, we have _more complex state_ - for example if it has multiple states, multiple ways of changing it or dependencies to other states.

```js
const [state, dispatchFn] = useReducer(reducerFn, initialState);
const reducerFn = (state, action) => {}; // returns updated state value
```

## React Context

- Component-wide "behind the scenes" state storage
- Creating a context

```js
import React from "react";

const AuthContext = React.createContext({
  isLoggedIn: false,
});

export default AuthContext;
```

- To make this context available in React component, wrap the JSX around which you need to access this context with `<AuthContext.Provider></AuthContext.Provider>`
- We can pass in a value component inside this AuthContext.Provider component which determines the values of the different properties of the context.
- To use this context in a component, you can call:

```js
const ctx = useContext(AuthContext);
```

- React Context is **NOT optimized** for high frequency changes.
