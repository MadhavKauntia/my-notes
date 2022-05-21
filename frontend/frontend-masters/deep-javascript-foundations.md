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

> A quality JS program embraces coercions, making sure the types involved in every operation are clear. Thus, corner cases are safely managed.

## Equality

- **==** allows coercion (types different)
- **===** disallows coercions (types same)

**==** Summary:

- If the types are same -> ===
- If _null_ or _undefined_ -> equal
- If non-primitive -> toPrimitive()
- Prefer: toNumber()

Avoid:

- **==** with 0 or ""
- **==** with non-primitives
- **== true** or **== false**: allow toBoolean or use **===**

## Scope

- All lexcial scopes are determined at compile-time, not runtime.
- Variables have two functions - setting value (target) or receiving value (source).
- During compile-time, all the variables are assigned scopes based on their positions. This is done by the compiler and the scope manager.
- During runtime, if a variable is not found within a scope, then a lookup to the parent scope happens.

```js
var teacher = "Kyle";

function otherClass() {
  teacher = "Suzy";
  topic = "React";
  console.log("Welcome!");
}

otherClass(); // Welcome!

teacher; // Suzy
topic; // React
```

In this case, since `topic` is not defined in the scope of `otherClass` or the global scope, ideally, we should get an error. However, in JavaScript, if you try to assign to a variable that's never been formally declared, once you arrive at the global scope, the variable is created by JS in the global scope. This happens when JS is running in not-strict mode. When JS is running in strict mode, that line would throw a `ReferenceError`.

```js
var teacher = "Kyle";

function otherClass() {
  var teacher = "Suzy";

  functionask(question) {
    console.log(teacher, question);
  }
  ask("Why?");
}

otherClass(); // Suzy Why?
ask("???"); // ReferenceError
```

**IIFE Patterns** - If there is a global variable which is being used somewhere further down in the code, and another developer accidentally overrides the variable somewhere in between, that would break the old code. A way to prevent this is by using the IIFE Pattern.

```js
var teacher = "Kyle";

(function anotherTeacher() {
  var teacher = "Suzy";
  console.log(teacher); // Suzy
})();

console.log(teacher); // Kyle
```

**Hoisting** - Hoisting is JavaScript's default behavior of moving declarations to the top. In JS, variables can be declared after it has been used.

The exception lies with the `let` and `const` variables. These variables are hoisted to the top of the block, but not initialized. These variables are in the _temporal dead zone_ from the start of the block until it is declared.

```js
x = 5;
console.log(x); // 5
var x;

carName = "Swift"; // ReferenceError
carBrand = "Maruti"; // ReferenceError
let carName;
const carBrand;
```

> Function declarations are hoisted but function expressions are not hoisted because assignment is a runtime operation.
