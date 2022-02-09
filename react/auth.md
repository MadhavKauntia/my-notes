# Authentication

* **Server-side Sessions**: Once a server grants access, it generates a unique identifier. It stores this unique identifier on the server and sends the same to the client. Client sends identifier along with requests to protected resources.
* **Authentication Tokens**: The server creates (but does not store) a permission token and sends this token to the client. The key to decode this token is only known by the server.

## Sample Auth Contxt
```js
import React, { useState } from 'react';

const AuthContext = React.createContext({
  token: '',
  isLoggedIn: false,
  login: (token) => {},
  logout: () => {},
});

export const AuthContextProvider = (props) => {
  const [token, setToken] = useState(null);

  const userIsLoggedIn = !!token;

  const loginHandler = (token) => {
    setToken(token);
  };

  const logoutHandler = () => {
    setToken(null);
  };

  const contextValue = {
    token: token,
    isLoggedIn: userIsLoggedIn,
    login: loginHandler,
    logout: logoutHandler,
  };

  return (
    <AuthContext.Provider value={contextValue}>
      {props.children}
    </AuthContext.Provider>
  );
};

export default AuthContext;
```

## Protecting Frontend Pages

To prevent users from accessing certain routes unless they're authenticated, we can simply declare routes conditionally.

```js
{!authCtx.isLoggedIn && (
    <Route path="/auth">
        <AuthPage />
    </Route>
)}

{authCtx.isLoggedIn && (
    <Route path="/profile">
        <UserProfile />
    </Route>
)}
```

## Persisting the User Authentication Status
* We can persist the authentication status of the user by storing the token in the local storage and clearing the token whenever the user logs out.
* To add auto-logout feature, we can persist the token expiration time in the local storage.