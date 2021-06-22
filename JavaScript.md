# JavaScript

## Basics

- In [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript), a **primitive** (primitive value, primitive data type) is data that is not an [object](https://developer.mozilla.org/en-US/docs/Glossary/Object) and has no [methods](https://developer.mozilla.org/en-US/docs/Glossary/Method). There are 7 primitive data types: [string](https://developer.mozilla.org/en-US/docs/Glossary/String), [number](https://developer.mozilla.org/en-US/docs/Glossary/Number), [bigint](https://developer.mozilla.org/en-US/docs/Glossary/BigInt), [boolean](https://developer.mozilla.org/en-US/docs/Glossary/Boolean), [undefined](https://developer.mozilla.org/en-US/docs/Glossary/undefined), [symbol](https://developer.mozilla.org/en-US/docs/Glossary/Symbol), and [null](https://developer.mozilla.org/en-US/docs/Glossary/Null).

### Function Types

#### Legacy Function (ES5)

```javascript
const nameOfFunction = function (argument) {
    doSomething();
}
```

#### Arrow Function (ES6)

```javascript
const nameOfFunction = () => {
    doSomething();
}
```

### Map Function

```javascript
array = [1, 2, 3];
array.map(function (number) {
    return number * 2;
})
```

## Path

### Importing Path

```javascript
const path = require('path')
```

### Create Path

- `__dirname` provides the path of the directory where the current file resides.
-  It provides different OS support.

```javascript
const newPath = path.resolve(__dirname, "folder", "filename.txt")
```

## File System

### Importing `fs`

```javascript
const fs = require('fs') // Simple fs
const fs = require('fs-extra') // More function fs
```

### Removing directory

- It also removes all it’s sub folders and sub files.
- It is a new function of `fs-extra`.

```javascript
fs.removeSync(newPath)
```

### Reading File

```javascript
const fileContents = fs.readFileSync(newPath, 'utf8')
```

### Ensuring Directory

- Create a directory if it not exists.

```javascript
fs.ensureDirSync(folderPath);
```

### Writing File

```javascript

```

### Spread Operator (…)

Assume you have the following object:

```javascript
const adrian = {
    fullName: 'Adrian Oprea',
    occupation: 'Software developer',
    age: 31,
    website: 'https://oprea.rocks'
};
```

Let’s assume you want to create a new object(person) with a different name and website, but the same occupation and age.

You could do this by specifying only the properties you want and use the spread operator for the rest, like below:

```javascript
const bill = {
    ...adrian,
    fullName: 'Bill Gates',
    website: 'https://microsoft.com'
};
```

What the code above does, is to spread over the `adrian` object and get all its properties, then overwrite the existing properties with the ones we're passing. 		Think of this spread thing as extracting all the individual properties one by one and transferring them to the new object.

Another Example with an array

```javascript
const numbers1 = [1, 2, 3, 4, 5];
const numbers2 = [ ...numbers1, 1, 2, 6,7,8]; // this will be [1, 2, 3, 4, 5, 1, 2, 6, 7, 8]
```



### Rest Operator or Parameter

The **rest parameter** syntax allows a function to accept an indefinite number of arguments as an array, providing a way to represent [variadic functions](https://en.wikipedia.org/wiki/Variadic_function) in 	JavaScript.

```javascript
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current;
  });
}

console.log(sum(1, 2, 3));
// expected output: 6

console.log(sum(1, 2, 3, 4));
// expected output: 10
```

### The arguments object

**`arguments`** is an `Array`-like object accessible inside [functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions) that contains the values of the arguments passed to that function.

```javascript
function func1(a, b, c) {
  console.log(arguments[0]);
  // expected output: 1

  console.log(arguments[1]);
  // expected output: 2

  console.log(arguments[2]);
  // expected output: 3
}

func1(1, 2, 3);
```

### Reducer

- Reducer changes the state on the basis of previous state and an action.

- A reducer is a function which takes two arguments — the current state and an action — and returns based on both arguments a new state.

- A Reducer Function is a perfect example of a Pure Function.

- We can express the idea in a single line, as an almost valid function:

  ```javascript
  const reducer = (state, action) => newState;
  ```

- Building a Counter using Reducer:

  ```javascript
  // state : An object to store the states
  // E.g. state = { count : 23 }, state = { count : 0, flag : true }
  
  // action : An object to store the different types of actions the reducer can perform
  // E.g. action = { type: ‘INCREMENT’ }, action = { type: ‘DECREMENT’ }
  
  function counterReducer(count, action) {
    // Prefer switch case to make the code concise, because the number of actions can be many 
    switch (action.type) {
      case "INCREASE":
        return { ...state, count: state.count + 1 }; // Copy previous state and override the specified one
      case "DECREMENT":
        return { ...state, count: state.count - 1 };
      default:
        return state;
    }
  }
  
  // Using the Reducer
  counterReducer({ count : 0 }, { type: ‘INCREMENT’ }) // 1
  counterReducer({ count : 1 }, { type: ‘DECREMENT’ }) // 0
  ```


- We can use payload within the action object, to provide the new information for the states. Here’s an exmaple:

  ```javascript
  const initialState = {
    name: "Mark",
    email: "mark@gmail.com",
  };
  
  function userReducer(state, action) {
    switch (action.type) {
      case "CHANGE_NAME":
        return { ...state, name: action.payload.name };
      case "CHANGE_EMAIL":
        return { ...state, email: action.payload.email };
      default:
        return state;
    }
  }
  
  const action = {
    type: "CHANGE_EMAIL",
    payload: { email: "mark@compuserve.com" },
  };
  
  userReducer(initialState, action); // {name: "Mark", email: "mark@compuserve.com"}
  ```

  
