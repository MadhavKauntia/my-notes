# Redux

A state management system for cross-component or app-wide state.

## Context vs Redux
- Context can have a complex setup.
- Context is not very efficient for high frequency updates.

## Core Redux Concepts
- Central Data (State) Store - Redux maintains a Central Data Store. Components set up subscriptions to this Store. Whenever the data changes, the Store notifies the components and the components can then fetch the updated data.
- Reducer Function - Components never directly change data in the Store. The Reducer Function is used to mutate the Store Data.
- Action - Components dispatch certain actions which describe the operation which the Reducer must perform.

An example of Redux using vanilla JS.
```js
const redux = require('redux');

const counterReducer = (state = {counter: 0}, action) => {
    if(action.type === 'increment') {
        return {
            counter: state.counter + 1
        };
    }
    if(action.type === 'decrement') {
        return {
            counter: state.counter - 1
        };
    }
    return state;
};

const store = redux.createStore(counterReducer);

const counterSubscriber = () => {
    const latestState = store.getState();
    console.log(latestState);
};
store.subscribe(counterSubscriber);

store.dispatch({ type: 'increment'});
store.dispatch({ type: 'decrement'});
```

To use Redux in a React application, we create a store folder in which we store a file. This file contains the Store declaration and the reducer function it points to, and exports the store.
```js
import { createStore } from 'redux';

const counterReducer = (state = { counter: 0 }, action) => {
    if (action.type === 'increment') {
        return {
            counter: state.counter + 1
        };
    } else if (action.type === 'decrement') {
        return {
            counter: state.counter - 1
        };
    }
    return state;
};

const store = createStore(counterReducer);

export default store;
```

Then, in the main ```index.js``` file, we import ```{ Provider }``` from 'react-redux' and enclose ```<App />``` within ```<Provider></Provider>```. We also import the store from our original file and pass it to the Provider.
```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';

import './index.css';
import App from './App';
import store from './store/index'

ReactDOM.render(<Provider store={store}><App /></Provider>, document.getElementById('root'));
```

Finally, to fetch the state or to dispatch actions in the components, we can use useSelector and useDispatch hooks in the 'react-redux' library in the following way:
```js
  const dispatch = useDispatch();
  const counter = useSelector(state => state.counter);
  const incrementHandler = () => {
    dispatch({ type: 'increment' });
  }
  const decrementHandler = () => {
    dispatch({ type: 'decrement' });
  }
```
* Never mutate an existing state in the reducer function. Always override the existing state with a new state.

## Redux Toolkit

> npm install @reduxjs/toolkit

A "slice" is a collection of Redux reducer logic and actions for a single feature in your app, typically defined together in a single file. The name comes from splitting up the root Redux state object into multiple "slices" of state.

```js
const counterSlice = createSlice({
    name: 'counter',
    initialState: {counter: 0, showCounter: true},
    reducers: {
        increment(state) {
            state.counter++; // we can modify state directly in slices
        },
        decrement(state) {
            state.counter--;
        },
        increase(state, action) {
            state.counter = state.counter + action.payload;
        },
        toggleCounter(state) {
            state.showCounter = !state.showCounter;
        }
    }
});

const store = configureStore({
    reducer: {counter: counterSlice.reducer} // can add multiple slices here as key-value pairs, use the keyname while fetching state value in components (state.key.stateValue)
});

export const counterActions = counterSlice.actions;
export default store;
```

To use an action in dispatch, we import counterActions and then call using ```dispatch(counterActions.increase(5)))```.

Each slice can be created in a new file and then we import all slices in a common file and create the store.