# Advanced Redux

Reducers must be pure, side-effect free, synchronous functions.
Side effects and async tasks must be executed:
1. Inside the components (by using useEffect) - This can be done by fetching the redux state using useSelector and using it as a dependency in useEffect in any component to perform an async task whenever the state is updated.
2. Inside the **action creators**

* **Thunk** - An action creator function that does not return the action itself but another function which eventually returns the action.

**How to create a Thunk?**
In the slice file, we can add a new function which behaves as a Thunk.

```js
export const sendData = (data) => {
    return async (dispatch) => {
        // perform async functions here or dispatch other reducers
    }
}
```

This Thunk can then be used in a Component.

```js
const dispatch = useDispatch();
dispatch(sendData(data));
```