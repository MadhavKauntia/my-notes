## Portals

- Everything in React is rendered in the root div specified in the `index.html` file.
- Sometimes when we use overlays and backdrops, it does not make sense to have all the HTML components in a single div.
- Using React portals, we can create a separate `<div>` in the index.html file and then render components inside it using the following code.

```js
import ReactDOM from "react-dom";

const Modal = (props) => {
  return (
    <>
      {ReactDOM.createPortal(<Component />, document.getElementById("div-id"))}
    </>
  );
};
```

## Refs

- We can use refs to connect our html components with js code.
- Refs can be used as an alternative to useState too.

```js
userNameRef = useRef();

return (<>
    <div ref={userNameRef}>
</>)
```

- Use useState() when we have to often update the state and use refs when you only mostly read the value.

We can extract or set the value of the userNameRef variable using `userNameRef.current.value`. However, we should not modify the data using refs as this manipulates the DOM which is not ideal. Only React should be the one manipulating the DOMs.
