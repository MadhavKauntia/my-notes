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

## Prompt
Useful to generate a prompt. For example, when user tries to navigate away while entering a form, he should be asked to confirm if he wants to leave because the form data might get lost.

```js
import { Prompt } from 'react-router-dom';

// Component definition and everything

<Prompt when={isEntering} message={(location) => 'Are you sure you want to leave?'} />
```

## Query Params

To fetch query params in a URL in a Component.

```js
import { useLocation } from 'react-router-dom';

const location = useLocation();
const queryPrams = new URLSearchParams(location.search); // this object now contains all query params as key-value pairs
```

## Using Match

It is not good practice to hardcode the Route path in all the components. If the path requires a change in the future, this will require a change in all the components. Instead, we can use the hook ```useRouteMatch`` to extract the current path and append to it.

```js
import { useRouteMatch } from 'react-router-dom';

const match = useRouteMatch();

<Route path={`${match.path}/comments`}>
    <Comments />
</Route>
// can also use match.url to extract the URL in case it contains a dynamic path variable
```

## An alternative history usage

```js
history.push({
    pathname: location.pathname, // path
    search: `?sort=asc` // query params
});
```

## React Router v6
* Replace ```<Switch>``` with ```<Routes>```.
* Instead of specifying the component to be rendered as a child of ```<Route>```, we now pass it as element attribute in JSX format.
```js
<Route path='/welcome' element={<Welcome />} />
```
* React Router always looks for exact path matches now. So, we do not need to specify **exact** property.
* NavLink does not support activeClassName.
```js
<NavLink className={(navData) => navData.isActive ? classes.active : ''} to='/welcome'>
    Welcome
</NavLink>
```
* **Redirect** has been replaced as **Navigate**.
* If a Component has nested Routes, declare it as '/welcome/*' in the App.js or the base file. Also, the nested Routes now only need the additional path and not the base path (<Route path="new-user" element={<p>Hello</p>}>)
* Nested routes can now be specified in App.js as the children components of the parent Route. To specify where in the component must the nested component be rendered, use **Outlet**.
* **useHistory** has been replaced by **useNavigate**.
```js
const navigate = useNavigate();
navigate('/welcome', {replace: true}); // skip the second parameter if you don't want to redirect
```