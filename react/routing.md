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