# Redux

## Reducer Function

- A reducer is a function which takes two arguments — the current state and an action — and returns a new state.

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

- It stores the state of application when Web App is initialized.

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

### Sync vs Async Actions

#### Sync

- As soon as the action was dispatched, the state is updated.

- Example, `buyCake()`, `buyIceCream()`

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

#### Async

- We wait for a task to be completed before dispatching our actions.
- Example, Asynchronous API Calls to fetch data from an end point and use that data in your application.

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

### Middlewares

- Middlewares are like extensions to Redux.
- Some Popular Middlewares are:

  - `redux-logger` - Used to Log Redux State Updates
  - `redux-thunk` - Used to Handle Async Operations

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

- Middlewares are added while creating the Redux Store.

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

### Add Redux to React project

```zsh
yarn add react-redux
```

### Structuring Files

Inside the `src` folder -

```zsh
D:\Codes\Redux\react-redux-demo
├── public
|  └── favicon.ico
├── src
|  ├── components
|  |  ├── CakeContainer.js
|  |  ├── IceCreamContainer.js
|  |  └── UserContainer.js
|  ├── redux
|  |  ├── cake
|  |  |  ├── cakeActions.js
|  |  |  ├── cakeReducer.js
|  |  |  └── cakeTypes.js
|  |  ├── iceCream
|  |  |  ├── iceCreamActions.js
|  |  |  ├── iceCreamReducer.js
|  |  |  └── iceCreamTypes.js
|  |  |── user
|  |  |  ├── userActions.js
|  |  |  ├── userReducer.js
|  |  |  └── userTypes.js
|  |  ├── rootReducer.js
|  |  ├── store.js
|  |  └── index.js
|  ├── index.js
|  └── App.js
├── README.md
├── package.json
└── yarn.lock
```

The components directory contains all the React Components.

The redux folder has following files and folders

- `cake/`, `iceCream/`, `user/` (Reducer folder)

- `index.js` (Only exports the Action Creators)

- `rootReducer.js` (Combines all the Reducers)

- `store.js` (Creates a Redux Store to be used globally)

Each Reducer will have three files. For E.g. User Folder -

```zsh
└── user
   ├── userActions.js (Action Creators)
   ├── userReducer.js (Reducer Function)
   └── userTypes.js   (Action Types)
```

#### `userTypes.js`

- This file only stores the names of Actions as strings.
- It prevents typos and allows easy way to modify, import and export the Actions.

```javascript
export const FETCH_USERS_REQUEST = "FETCH_USERS_REQUEST";
export const FETCH_USERS_SUCCESS = "FETCH_USERS_SUCCESS";
export const FETCH_USERS_FAILURE = "FETCH_USERS_FAILURE";
```

#### `userActions.js`

- This file will store the Action Creators.
- Action Creators are the functions returning an action as an object.
- For Async processes like fetching data from api is handled by an action creator which dispatches other action creators.

```javascript
import {
  FETCH_USERS_REQUEST,
  FETCH_USERS_SUCCESS,
  FETCH_USERS_FAILURE,
} from "./userTypes";
import axios from "axios";

export const fetchUsersRequest = () => {
  return {
    type: FETCH_USERS_REQUEST,
  };
};

export const fetchUsersSuccess = (users) => {
  return {
    type: FETCH_USERS_SUCCESS,
    payload: users,
  };
};

export const fetchUsersFailure = (error) => {
  return {
    type: FETCH_USERS_FAILURE,
    payload: error,
  };
};

// Async Action Creator
export const fetchUsers = () => {
  return (dispatch) => {
    dispatch(fetchUsersRequest);
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((response) => {
        const users = response.data;
        dispatch(fetchUsersSuccess(users));
      })
      .catch((error) => {
        const errorMessage = error.message;
        dispatch(fetchUsersFailure(errorMessage));
      });
  };
};
```

#### `userReducer.js`

- The Reducer file will contain the `initialState` object and the reducer function.
- This reducer function will then get imported into a `rootreducer.js` file.

```javascript
import {
  FETCH_USERS_REQUEST,
  FETCH_USERS_SUCCESS,
  FETCH_USERS_FAILURE,
} from "./userTypes";

const initialState = {
  loading: false,
  users: [],
  error: "",
};

const userReducer = (state = initialState, action) => {
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
        users: [],
        error: action.payload,
      };
    default:
      return state;
  }
};

export default userReducer;
```

The other redux files are structured as follows.

```zsh
├── redux
|  ├── cake
|  |  ├── cakeActions.js
|  |  ├── cakeReducer.js
|  |  └── cakeTypes.js
|  ├── iceCream
|  |  ├── iceCreamActions.js
|  |  ├── iceCreamReducer.js
|  |  └── iceCreamTypes.js
|  |── user
|  |  ├── userActions.js
|  |  ├── userReducer.js
|  |  └── userTypes.js
|  ├── index.js
|  ├── rootReducer.js
|  └─── store.js
```

- The first three folders contain different reducers and their supporting files.
- The three other files are used in combining reducers, creating Redux Store and a file used to export action creators.

#### `rootReducer.js`

- This file combines all the Reducers inside a project.
- In case you have combined multiple reducres, make sure to edit the new name changes in all the Components.
- Initially when we have one reducer we can receive the state like `state.numOfCakes` but when combining multiple reducers and using a root reducer we retrieve states like `state.cake.numOfCakes`.
- The syntax will be like `state.{name-of-reducer}.{name-of-state}`.

```javascript
// Combines all the Reducers

import { combineReducers } from "redux";
import cakeReducer from "./cake/cakeReducer";
import iceCreamReducer from "./iceCream/iceCreamReducer";
import userReducer from "./user/userReducer";

const rootReducer = combineReducers({
  cake: cakeReducer,
  iceCream: iceCreamReducer,
  user: userReducer,
});

export default rootReducer;
```

#### `store.js`

- This file creates a Redux Store for your whole project.
- You can add the middlewares inside this file only.
- You can also add `composeWithDevTools`, to make Redux Devtools extension work with the Web App.

```javascript
// Creates a Redux Store to be used globally

import { createStore, applyMiddleware } from "redux";
import logger from "redux-logger";
import thunk from "redux-thunk";
import rootReducer from "./rootReducer";
import { composeWithDevTools } from "redux-devtools-extension";

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(logger, thunk))
);

export default store;
```

#### `index.js`

- This file is used to export the Action Creators, so that we can import them inside any component.
- You can use `*` to export all the Action Creators inside the file.

```javascript
// Only exports the Action Creators

export { buyCake } from "./cake/cakeActions";
export { buyIceCream } from "./iceCream/iceCreamActions";
export * from "./user/userActions";
```

Now, when you're done with all the reducers and creating the store, it's time to integrate it with the React Web App.

```zsh
React-Redux-Project
├── public
|  └── ...
├── src
|  ├── components/
|  |  └── ...
|  ├── redux/
|  |  └── ...
|  ├── App.js
|  └── index.js
├── package.json
└── README.md
```

We have this global `App.js` file where we import all our components. You can add the Redux store provider here to make the Redux store work anywhere inside the Web App.

#### `App.js`

- The global Project file where all the components are imported.

```javascript
import React from "react";
import { Provider } from "react-redux";
import store from "./redux/store";
import "./App.css";

import CakeContainer from "./components/CakeContainer";
import ItemContainer from "./components/ItemContainer";
import UserContainer from "./components/UserContainer";

function App() {
  // Makes the Redux store available to the connect() calls in the component hierarchy below.
  return (
    <Provider store={store}>
      <div className="App">
        {/* <CakeContainer /> */}
        {/* <ItemContainer cake /> */}
        {/* <ItemContainer /> */}
        <UserContainer />
      </div>
    </Provider>
  );
}

export default App;
```

The below code shows how to connect the component with Redux

#### Connect React Components with Redux

- It requires creating two functions `matchStateToProps` and `matchDispatchToProps`.

- We then connect the component with these two functions, so that we can receive the state and dispatch function inside the component as a prop.

```javascript
import React, { useEffect } from "react";
import { connect } from "react-redux";
import { fetchUsers } from "../redux";

const UserContainer = ({ userData, fetchUsers }) => {
  useEffect(() => {
    fetchUsers();
  }, []);

  return userData.loading ? (
    <h2>Loading...</h2>
  ) : userData.error ? (
    <h2>{userData.error}</h2>
  ) : (
    <div>
      <h2>User List</h2>
      <div>
        {userData &&
          userData.users &&
          userData.users.map((user) => <p>{user.name}</p>)}
      </div>
    </div>
  );
};

// Global state created by Redux
const mapStateToProps = (state) => {
  return {
    userData: state.user,
  };
};

// Dispatch function by Redux for activating action creators.
const mapDispatchToProps = (dispatch) => {
  return {
    fetchUsers: () => dispatch(fetchUsers()),
  };
};

// The connect function that binds everything together
export default connect(mapStateToProps, mapDispatchToProps)(UserContainer);
```

We can also send the payload to the Action Creators by asking information from the user.

```javascript
import { BUY_CAKE } from "./cakeTypes";

// Pass 1 as default to add the ability to dispatch without argument
export const buyCake = (number = 1) => {
  return {
    type: BUY_CAKE,
    payload: number,
  };
};
```

#### Connect React Component to Redux (Classic)

```javascript
import React, { useState } from "react";
import { connect } from "react-redux";
import { buyCake } from "../redux";

const NewCakeContainer = (props) => {
  const [number, setNumber] = useState(1);

  return (
    <div>
      <h2>Number of Cakes - {props.numOfCakes}</h2>
      <input
        type="text"
        value={number}
        onChange={(event) => setNumber(parseInt(event.target.value))}
      />
      <button onClick={() => props.buyCake(number)}>Buy {number} Cake</button>
    </div>
  );
};

const mapStateToProps = (state) => {
  return {
    numOfCakes: state.cake.numOfCakes,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    buyCake: (number) => dispatch(buyCake(number)),
  };
};

// connect() connects the React Component to Redux Store
export default connect(mapStateToProps, mapDispatchToProps)(NewCakeContainer);
```

#### Connect React Component to Redux (Hooks)

```javascript
import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { buyCake } from "../redux";

/*
    The React Redux Docs say to use React Redux Hooks over the connect(), but there are a few Usage Warnings.
    Usage Warnings - https://react-redux.js.org/api/hooks#usage-warnings
*/

const HooksCakeContainer = () => {
  // Below function works similar to the mapStateToProps() in CakeContainer.js
  // const numOfCakes = useSelector(state => state.numOfCakes)

  // Multiple Reducers
  const numOfCakes = useSelector((state) => state.cake.numOfCakes);

  // Dispatch function takes in Action Creator
  const dispatch = useDispatch();

  return (
    <div>
      <h2>Num of Cakes - {numOfCakes}</h2>
      <button onClick={() => dispatch(buyCake())}>Buy Cake</button>
    </div>
  );
};

export default HooksCakeContainer;
```

#### Combining two React Components and their Actions

- In the below example we'll connect the Cake and Ice Cream components and actions into one component called Item.
- We use `ownProps` argument in `mapDispatchToProps` to differentiate between which state to change.
- The benefit of using this method is that we'll have to create single component instead of creating different components. For example, using item instead of Cake and Ice Cream.

```javascript
import React from "react";
import { connect } from "react-redux";
import { buyCake, buyIceCream } from "../redux";

const ItemContainer = (props) => {
  return (
    <div>
      <h2>Item - {props.item}</h2>
      <button onClick={props.buyItem}>Buy Items</button>
    </div>
  );
};

const mapStateToProps = (state, ownProps) => {
  const itemState = ownProps.cake
    ? state.cake.numOfCakes
    : state.iceCream.numOfIceCreams;

  return {
    item: itemState,
  };
};

const mapDispatchToProps = (dispatch, ownProps) => {
  const dispatchFunction = ownProps.cake
    ? () => dispatch(buyCake())
    : () => dispatch(buyIceCream());

  return {
    buyItem: dispatchFunction,
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(ItemContainer);
```

To use it we'll have to pass a prop while adding the Component in `App.js`

```javascript
// When we want to alter the state of Cake only
<ItemContainer cake>

// When we want to alter the state of iceCream only
<ItemContainer iceCream>
```

## Learn More

- [Redux.js Official Documentation](https://redux.js.org/introduction/getting-started)
- [React Redux Tutorial - Codevolution](https://www.youtube.com/watch?v=9boMnm5X9ak&list=PLC3y8-rFHvwheJHvseC3I0HuYI2f46oAK)