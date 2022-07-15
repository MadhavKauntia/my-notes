# [TypeScript Fundamentals, v3](https://www.typescript-training.com/course/fundamentals-v3)

## Setup

tsconfig.json

```json
{
  "compilerOptions": {
    "outDir": "dist", // where to put the ts files
    "target": "ES3" // which level of JS support to target
  },
  "include": ["src"] // which files to compile
}
```

- In TypeScript, variables are _born_ with their types. There is no safe way to change a variable's type from `number` to `string`.
- If TS cannot determine the type of a variable yet, it is assigned the type `any`.

## Type Categories

### Objects

Object types are defined by:

- name of the properties
- types of those properties

```js
let car: {
  make: string
  model: string
  year: number
  chargeVoltage?: number // ? denotes this property is optional
};
```

- If we don't declare all mandatory properties or declare an extra property in an object, TS will display an error.

### Index Signature (to define type of dictionaries)

```js
const phones: {
    [k: string]: {
        country: string
        area: string
        number: string
    }
} = {};
```

### Arrays

```js
const fileExtensions: string[] = ["js", "ts"];
```

### Structural vs Nominal Types

- Nominal types are all about names. In Java, when we pass an object as an argument in a function, it will validate if the function parameter and the argument belong to the same class.

- Structural types are more about structures. Even if the type of an argument object being passed in the function is different from the type of the parameter being taken by the function, as long as it has all the required properties, it will not show an error.

### Union Types and Intersection Types

- Union types in TypeScript can be described using the pipe operator. For example, if we have a type that could be one of two strings, `success` or `error`, we could define it as `"success" | "error"`.

- Intersection types in TS can be described using the `&` operator.

```ts
function makeWeek(): Date & { end: Date } {
  const start = new Date();
  const end = new Date(start.valueOf() + ONE_WEEK);

  return { ...start, end };
}
```

### Type Aliases

Type aliases help with the following:

- they allow us to give our type a more meaningful name
- declare the particulars of a type in a single place
- import and export the type from modules, the same as if it were an exported value

```ts
type UserContactInfo = {
  name: string;
  email: string;
};
```

## Functions

## Function Overloading

```ts
type FormSubmitHandler = (data: FormData) => void;
type MessageHandler = (evt: MessageEvent) => void;

function handleMainEvent(
  elem: HTMLFormElement | HTMLIFrameElement,
  handler: FormSubmitHandler | MessageHandler
) {}
```

### Class Definition

```ts
class Car {
  make: string;
  mdoel: string;
  year: number;
  constructor(make: string, model: string, year: number) {
    this.make = make;
    this.model = model;
    this.year = year;
  }
}
```

The above code can be re-written as:

```ts
class Car {
  constructor(
    public makeL string,
    public model: string,
    public year: number
  ) {}
}
```

## Types and Values

- `never` is a type which cannot hold any value, it is useful for exhaustive conditionals.

We can create an Error subclass to handle errors gracefully:

```ts
class UnreachableError extends Error {
  constructor(_nvr: never, message: string) {
    // we pass in the variable which is causing the error as _nvr, if it has a value, we get compile time errors
    super(message);
  }
}
```

- **Type Guards** can be used to write a function which checks if a variable is of a user-defined type.
