# Javascript: The Hard Parts

1. Execution Context: The Execution Context contains the code that's currently running, and everything that aids in its execution. During the Execution Context run-time, the specific code gets parsed by a parser, the variables and functions are stored in memory, executable byte-code gets generated, and the code gets executed.

2. Call Stack:
   - JS keeps track of what function is currently running (where is the thread of execution)
   - Run a function -> add to call stack
   - Finish running the function -> JS removes it from call stack
   - Whatever is top of the call stack, that's the function we're currently running

## Higher Order Functions

- A higher order function is a function that takes a function as an argument, or returns a function.
- This is possible becayse functions in javascript are basically first class objects. They can be treated like any other javascript object.
- The function we insert into a higher order function is called the **callback function**.

## Closure

### Functions with Memories

- When our functions get called, we create a live store of data for that function's execution context
- When the function finishes executing, its local memory is deleted (except the returned value)
- If our function could hold on to live data between executions, this would let the function definition have an associate cache or persistent memory
- It all starts with us **returning a function from another function**

### Backpack

- When we return a function from another function, we get more than just the code of that function.
- That function takes with it all the surrounding data in its _backpack_.
- Whenever we run that function, if we do not find resources in its local memory, we then look into its backpack.
- If we console.log that function, we will see a hidden property `[[scope]]`. This hidden property persists whenever a function is returned.

```js
function outer() {
  let counter = 0;
  function incrementCounter() {
    counter++;
  }
  return incrementCounter;
}

const myNewFunction = outer();
myNewFunction(); // 1
myNewFunction(); // 2

const anotherFunction = outer();
anotherFunction(); // 1
anotherFunction(); // 2
```

In the above example, each time `outer()` is called and `incrementCounter()` is returned, the returned function gets a new backpack because it was created in a new execution context.

## Asynchronous JavaScript

A lot of JavaScript features are actually not JS features but web browser features.

DOM -> document
Network Requests -> fetch
Timer => setTimeout

These functions are not executed in JavaScript but in a web browser. Coined term for these functions - **facade functions**.

Until ES6, the way asynchronous JavaScript worked was as follows:

- Suppose we call `setTimeout(myFunction, 1000)` and then a million console.log statements after that.
- After 1000ms, the browser will add the function `myFunction` to the **Callback Queue**.
- After executing every line of code, the **event loop** checks if there is still something left in the call stack or if there are still remaining lines to execute in global.
- If not, it adds `myFunction` from the callback queue to the call stack.

## Promises

Using two-pronged facade functions that both:

- Initiate background web browser work, and
- return a placeholder object (promise) immediately in JavaScript

The returned object has two properties - `value` and `onFulfilled`.
When the processing is complete, the final value is stored in `value`.

Using the `then()` method, we can add methods to the `onFulfilled` array. These functions will be executed on the `value` once it has finished execution.

There is a separate queue called the **microtask queue** to store the functions in the `onFulfilled` array. Basically, once the `value` is returned, the function to be performed is added to the microtask queue. This queue is checked after the call stack is empty but before the callback queue is checked.

Summary: Hold promise-deferred functions in a microtask queue and callback functions in a task queue (callback queue) when the Web Browser Feature finishes. We add the function to the call stack when the call stack is empty and all global code have run. Prioritize functions in the microtask queue over the callback queue.

## Classes and Prototypes

Suppose we want an object and its functions to be reusable, the problem with that is that the function we declare inside each object will violate the DRY rule since it will be created for every separate object. This is where **prototype** comes into play.

```js
function userCreator(name, score) {
  const newUser = Object.create(userFunctionStore);
  newUser.name = name;
  newUser.score = score;
  return newUser;
}

const userFunctionStore = {
  increment: function () {
    this.score++;
  },
  login: function () {
    console.log("Logged in");
  },
};

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment();
```

`Object.create()` always initializes an empty object, but it has a hidden property `__proto__` which stores a link to the `userFunctionStore` in this case. If a function/property is invoked which is not present in the parent function, JavaScript looks into this hidden property to find that property.

All objects have a `__proto__` property which points to `Object.prototype` by default unless it is overridden by `Object.create()`.

Whenever we use the `new` keyword to initialize a new object, all this is happening behind the scenes.
A function is both a function and an object. Whenever we use the `new` keyword the initialize an object, the function which is called also has a `prototype` property (empty object by default) which can be used to declare the common functions which that object can call or have a reference to.

```js
functio nuserCreator(name, score) {
  this.name = name;
  this.score = score;
}

userCreator.prototype.increment = function() {
  this.score++;
}
userCreator.prototype.login = function() {
  console.log('login');
}

const user1 = new userCreator("Eva", 9);

user1.increment();
```

We can use the `class` keyword to declare a function which needs to be initialized using a `new` keyword.
It is a `syntactic sugar` as it does not affect the functionality in any way.

- The function bit is called the constructor
- The prototype functions can be simply listed out inside the class
