# Facts about React

## JavaScript Basics

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

