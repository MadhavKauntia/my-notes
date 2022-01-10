# Javascript Refresher

## let & const
* let - variable values
* const - constant values

## Arrow Functions
```
const myFnc = () => {
    ...
}
```

## Exports and Imports
* default export
```
import person from './person.js'
import prs from './person.js'
```
* named export
```
import {smth} from './utility.js'
import * as bundled from './utility.js'
```

## Spread and Rest Operators
* **Spread** - used to split up array elements or object properties
```(
const newArray = [...oldArray, 1, 2]
const newObject = {...oldObject, newProp:5 }
```
* **Rest** - Used to merge a list of function arguments into an array
```
funvtion sortArgs(...args) {
    return args.sort();
}
```

## Destructuring
Allows to easily extract array elements or object properties and store them in variables.
* Array Destructuring
```
[a, b] = ['Hello', 'Max']
console.log(a) //Hello
console.log(b) //Max
```
* Object Destructuring
```
{name} = {name: 'Max', age:28}
console.log(name) //Max
console.log(age) //undefined
```