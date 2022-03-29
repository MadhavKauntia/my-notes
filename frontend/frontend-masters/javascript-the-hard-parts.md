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
