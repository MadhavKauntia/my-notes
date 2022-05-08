# Deep JavaScript Foundations v3

## Types

- It is a common misconception that all variables in JS are objects. JS also has primitive types: string, number, bigint, boolean, undefined, symbol, and null.

- Undeclared - The variable is not defined in any scope.
- Undefined - There is a variable but it does not contain a value yet.

- NaN - Represents an invalid number.

```js
var myAge = Number("0o46"); // 38
var myNextAge = Number("39"); // 39
var myCatsAge = Number("n/a"); // NaN
myAge - "my son's age"; // NaN

myCatsAge === myCatsAge; // false
```

- Object.is - It is an equality check which can be thought of as ====. It takes into consideration NaN and negative zero.
  Implementation:

```js
if (!Object.is) {
  Object.is = function ObjectIs(x, y) {
    var xNegZero = isItNegZero(x);
    var yNegZero = isItNegZero(y);

    if (xNegZero || yNegZero) {
      return xNegZero && yNegZero;
    } else if (isItNaN(x) && isItNaN(y)) {
      return true;
    } else {
      return x === y;
    }

    function isItNegZero(v) {
      return x === 0 && 1 / x === -Infinity;
    }

    function isItNaN(x) {
      return x !== x;
    }
  };
}
```

## Coercion

- toPrimitive(hint) - converts to primitive using the hint (type)/
- toString() - It takes any value and gives us the representation of that value in string form.
- toNumber() - Converts to number. It first checks valueoF() and then toString().
- toBoolean() - Falsy values - `"", 0, -0, null, NaN, false, undefined`. **ANY** other value will be truthy.
