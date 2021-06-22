# React

## Creating new project

### Using `npx` to create a new project

```zsh
npx create-react-app {my-app}
```

### Change the Directory

```zsh
cd {my-app}
```

### Start the project

```zsh
npm start
```

## Components

###  Functional Component

```javascript
import React from 'react';
const SearchBar = () => {
    return <input />
}
```

### Class Based Components

```javascript
import React, {Component} from 'react';
class SearchBar extends Component {
    render() {
        return <input />;
    }
}
```

## Event Handler

### Function in the class

```javascript
class SearchBar extends Component {
    render() {
        return < input onChange={this.onInputChange} />;
    }

    onInputChange(event) {
        console.log(event.target.value);
    }
}
```

### Inline

```javascript
class SearchBar extends Component {
    render() {
        return < input onChange={(event) => console.log(event.target.value)} />;
    }
}
```

## State

### Good Practice

```javascript
this.setState({ term: event.target.value }
```

### Bad Practice

```javascript 
this.state.term = event.target.value
```

## Controlled Element vs. Non-Controlled Element

| Controlled Element                                           | Non-Controlled Element                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| A controlled element has it's value changed only using the __state__. | A non controlled element's value changes as the user make any changes. |
| Any change user do => State changes => The changed state updates the value of element | Any change user do => Value Automatically Updates.           |

### Example

#### Controlled Element

  ```javascript
  < input 
      value={this.state.term}
      onChange={(event) => this.setState({ term: event.target.value })} 
  />
  ```

  #### Non-Controlled Element

```javascript
<input/>
```

### Submitting Form

- First Define the JSX in the `render()` function of the component class.

```jsx
<form onsSubmit={this.OnSubmit}>
    // Your Form Contents
</form>
```

- Then, create a new function exactly given below inside the component class only.

```javascript

```

The Anchor tag `<a></a>` gives us the right click functionality to open in a new tab.



## Lifecycle in React

- Mounting
  - When You first render that component.
- Updating
  - If you make an update to already rendered component.
  - A prop received by the component can cause the component to update and hence re-render.
  - A change in the state variable of a component can also trigger the update.
- Unmounting
  - If you decide not to display a component anymore.
  - When you hide a component or visit a different page.



## State Handling in Functional Components

- In functional components, we use `useState` and `useEffect` to handle state.

  ```javascript
  import React, { useState, useEffect } from 'react';
  ```

  ### The `useState` Hook

- The `useState` is used to initialize the name of the state and the function that will change the state.

  ```javascript
  const [count, setCount] = useState(0);
  ```

  Here, `count` is the name of the state, `setCount` is the function that’ll update the state. and the argument that is passed inside the `useState` (here `0`) is the initial state.

- Anything that’s passed inside the `setCount()` function as an argument will provide the method to update the state.

  ```jsx
  <button onClick={() => setCount(count + 1)}>
  ```

  Here, clicking on button will update the state variable `count` with `+1`.

  ### The `useEffect` hook

- Here is the implementation. You can use `useState` multiple times inside a functional component.

  ```javascript
  const FunctionalComponent = () => {
      const [count, setCount] = useState(0);
      
      useEffect(() => {
          // Do this whenever the component gets mounted or updated.
      })
      
      return(
      	// Render This
      )
  }
  ```

- If you want to make a change **only** when the component gets **mounted**, then pass an empty array along with the arrow function inside the `useEffect`.

  ```javascript
  const FunctionalComponent = () => {
      const [count, setCount] = useState(0);
      
      useEffect(() => {
          // Do this *only* when the component gets *mounted* 
          // because an empty array has also been passed
      }, [])
      
      return(
      	// Render This
      )
  }
  ```

- To handle unmounting, you can use `return` statement inside the arrow function of the `useEffect`.   Anything that’s inside the function that is returned will get executed when the component is unmounted.

  ```javascript
  const FunctionalComponent = () => {
      const [count, setCount] = useState(0);
      
      useEffect(() => {
          // Do this *only* when the component gets *mounted* 
          // because an empty array has also been passed
          
          return () => {
              // Do this when the component has unmounted.
          }
          
      }, [])
      
      return(
      	// Render This
      )
  }
  ```

  