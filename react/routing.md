# React Routing

> npm install react-router-dom@5

```js
index.js
--------
import {BrowserRouter} from 'react-router-dom';

ReactDOM.render(
    <BrowserRouter>
        <App />
    </BrowserRouter>,
    document.getElementById('root')
);
```

```js
App.js
------
import {Route} from 'react-router-dom';
import {Welcome, Products} from './components';

const App = () => {
    return <div>
        <Route path="/welcome">
            <Welcome />
        </Route>
        <Route path="/products">
            <Products />
        </Route>
    </div>;
};
```

## Linking between pages

```js
import { Link } from 'react-router-dom';

const Header = () => {
    return <div>
        <Link to="/welcome">Welcome</Link>
        <Link to="/products">Products</Link>
    </div>;
}
```

* NavLink works in the same way as Link. The additional benefit with NavLink is that it can be used to set a certain CSS class to the active component in navbar.

```js
<NavLink to='/welcome' activeClassName={classes.active}>Welcome</NavLink>
```

## Dynamic Routing

```js
<Route path="/products/:productId">
    <ProductDetail />
</Route>
```

```js
ProductDetail.js
----------------
import {useParams} from 'react-router-dom';

const ProductDetail = () => {
    const params = useParams();
    console.log(params.productId);
}
```

## Switch and exact
* Enclose your ```<Route>``` tags in ```<Switch>``` tags to tell react to only display one match at a time. Otherwise, React will render both ```/products``` and ```/products/:productId``` at the same time if we navigate to ```/products/p1``` because that's how React Router works.
* By default (with Switch enabled), React Router matches with the first path it finds, even if it's not an exact match. To tell React Router to match with a given path only if it is an exact match, use the ```exact``` property.
```js
<Route path="/products" exact>
    <Products />
</Route>
```

## Redirecting
```js
<Route path='/' exact>
    <Redirect to='/welcome'> //imported from 'react-router-dom'
</Route>
```

## Adding a Not Found page
* Create a NotFound page component.
* Add a ```<Route path="*"><NotFound /></Route>``` as the last Route in App.js

## Programmatic Navigation
* Action triggered by code after another action is completed
* Can be useful if we want to navigate user to page after login is complete

```js
import { useHistory } from 'react-router-dom';

const Component = () => {
    const history = useHistory();
    const actionHandler = () => {
        // perform the action

        history.push('/another-page'); // adds a new page, allows user to go back
        // OR
        history.replace('another-page'); // like a redirect where we change the current page, user cannot go back
    };
}
```