# Working with Forms and User Input

## Validation on Lost Focus

To validate a form input on lost focus, i.e, show error message if field is incorrect when input loses focus, we can use the `onBlur` attribute in the `<input>` tag and set it to a function which displays the error.

## Custom Hook to manage input states

```js
import { useState } from "react";

const useInput = (validateValue) => {
  const [enteredValue, setEnteredValue] = useState("");
  const [isTouched, setIsTouched] = useState(false);

  const valueIsValid = validateValue(enteredValue);
  const hasError = !valueIsValid && isTouched;

  const valueChangeHandler = (event) => {
    setEnteredValue(event.target.value);
  };
  const inputBlurHandler = (event) => {
    setIsTouched(true);
  };
  const reset = () => {
    setEnteredValue("");
    setIsTouched(false);
  };
  return {
    value: enteredValue,
    isValid: valueIsValid,
    hasError,
    valueChangeHandler,
    inputBlurHandler,
    reset,
  };
};

export default useInput;
```

- [Formik](https://formik.org/) is a great library to work with forms in React.
