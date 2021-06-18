# Redux

## Reducer Function

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

## Redux Core Concepts

### Action Types

- Action Types are saved as variables to prevent typos.
- It is a standard and is widely accepted by the community.

```javascript
// Basic Action Names
const BUY_CAKE = "BUY_CAKE";
```

- You can also save them in a JS file and export them as Named Exports
- These exported variables are mainly imported in the files where reducer function and action creators ares stored.

```javascript
// Async Action Names
export const FETCH_USERS_REQUEST = "FETCH_USERS_REQUEST";
export const FETCH_USERS_SUCCESS = "FETCH_USERS_SUCCESS";
export const FETCH_USERS_FAILURE = "FETCH_USERS_FAILURE";
```

### Action Creators

- Action Creators are the functions that returns an object containing the action type and payload.

```javascript
// Basic Action Creator
function buyCake() {
  return {
    type: BUY_CAKE,
  };
}

// Action Creator with Payload
export const fetchUsersSuccess = (users) => {
  return {
    type: FETCH_USERS_SUCCESS,
    payload: users,
  };
};
```

### Initial State

- Reducer function uses two arguments, `state` and `action`. For state argument a default value is provided known as `initialState`.

- It stores the state of application when Web App is initiated.

```javascript
// Sync Initial State

const initialState = {
  numOfCakes: 10,
  numOfIceCreams: 20,
};

// Async Initial State

const initialState = {
  loading: false,
  users: [],
  error: "",
};
```

### Reducer Functions (Sync vs Async)

- A reducer function takes `state` and `action` as arguments and always returns new state as an object.
- It can be visualized as `reducer = (state, action) => newState`

- An Example for Sync Reducer

```javascript
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case "BUY_CAKE":
      return {
        ...state, // Make a copy of all the elements of state
        numOfCakes: state.numOfCakes - 1, // Append Changes to this state
      };
    case "BUY_ICECREAM":
      return {
        ...state, // Make a copy of all the elements of state
        numOfIceCreams: state.numOfIceCreams - 1, // Append Changes to this state
      };
    default:
      return state;
  }
};
```

- An Example for Async Reducer

```javascript
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case FETCH_USERS_REQUEST:
      return {
        ...state,
        loading: true,
      };
    case FETCH_USERS_SUCCESS:
      return {
        ...state,
        loading: false,
        users: action.payload,
        error: "",
      };
    case FETCH_USERS_FAILURE:
      return {
        ...state,
        loading: false,
        error: action.payload,
      };
    default:
      return state;
  }
};

// For Async Reducers we create a special Action Creator which uses thunk Middleware
// This Action Creator dispatches other action creator.
// Thus we'll need to dispatch only one (below mentioned) Action Creator
const fetchUsers = () => {
  return function (dispatch) {
    dispatch(fetchUsersRequest());
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((response) => {
        // response.data is the array of the users
        const users = response.data.map((user) => user.id);
        dispatch(fetchUsersSuccess(users));
      })
      .catch((error) => {
        // error.message is the error description
        dispatch(fetchUsersFailure(error.message));
      });
  };
};
```

## Redux with Vanilla JS

### Add Redux to Project

```javascript
const redux = require("redux");
const createStore = redux.createStore;
```

### Create a Redux Store

```javascript
const store = createStore(reducer);
```

### Receive the State from redux

```javascript
console.log("State", store.getState());
```

### Subscribe to Redux

```javascript
store.subscribe(() => console.log("Updated state", store.getState()));
```

### Dispatch an Action

```javascript
// Using Action Creator (Convention)
store.dispatch(buyCake());
store.dispatch(fetchUsersSuccess(users));

// Directly Passing Object (Understanding)
store.dispatch({
  type: BUY_CAKE,
});
store.dispatch({
  type: FETCH_USERS_SUCCESS,
  payload: users,
});
```

### Combine two Reducers

- It is possible to combine multiple Reducers to one reducer known as `rootReducer`.

```javascript
const combineReducers = redux.combineReducers;

const cakeReducer = (state = initialCakeState, action) => {
 ...
}

const iceCreamReducer = (state = initialIceCreamState, action) => {
 ...
}

const rootReducer = combineReducers({
    cake: cakeReducer,
    iceCream: iceCreamReducer
})

// Don't forget to update the variable inside createStore
const store = createStore(rootReducer)
```

### Middlewares

- Middlewares are like extensions to Redux.
- Some Popular Middlewares are:

  - `redux-logger` - Used to Log Redux State Updates
  - `redux-thunk` - Used to Handle Async Operations

```javascript
// Import the function from Redux
const applyMiddleware = redux.applyMiddleware;

// Create a variable of your Middleware
const reduxLogger = require("redux-logger");
const logger = reduxLogger.logger(); // For Logger
const thunk = require("redux-thunk").default; // For thunk

// Applying Middleware
const store = createStore(rootReducer, applyMiddleware(logger, thunk));
```

### Unsubscribing to the Store

- You can unsubscribe to the store when all the dispatch actions are completed.

```javascript
const unsubscribe = store.subscribe(() =>
  console.log("Updated state", store.getState())
);
unsubscribe();
```

## React Redux

File Structure

Inside the `src` folder -

```zsh
src
├── App.js
├── components
|  ├── CakeContainer.js
|  ├── HooksCakeContainer.js
|  └── IceCreamContainer.js
├── redux
|  ├── cake
|  |  ├── cakeActions.js
|  |  ├── cakeReducer.js
|  |  └── cakeTypes.js
|  ├── iceCream
|  |  ├── iceCreamActions.js
|  |  ├── iceCreamReducer.js
|  |  └── iceCreamTypes.js
|  ├── index.js
|  ├── rootReducer.js
|  └── store.js
```

The components directory contains all the React Components.

The redux folder has following files and folders

- `cake/` (Reducer folder)

- `iceCream/` (Reducer folder)

- `index.js` (Only exports the Action Creators)

- `rootReducer.js` (Combines all the Reducers)

- `store.js` (Creates a Redux Store to be used globally)

Each Reducer will have three files. For E.g. Cake Folder -

```zsh
├── cake
|  ├── cakeActions.js (Action Creators are functions returning an object {type: BUY_CAKE})
|  ├── cakeReducer.js (Reducer Function)
|  └── cakeTypes.js (Exports type strings as a variable)
```

### Sync vs Async Actions

Sync =>

- As soon as the action was dispatched, the state is updated.

- Example, `buyCake()`, `buyIceCream()`

Async => - We wait for a task to be completed before dispatching our actions. - Example, Asynchronous API Calls to fetch data from an end point and use that data in your application.
