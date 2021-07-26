# Redux

## Overview
* [Data flow diagram](#data-flow-diagram)
* [State](#state)
* [Reducer](#reducer)
    * [Reducer rules](#reducer-rules)
    * [Splitting Reducers](#splitting-reducers)
    * [Combining Reducers](#combining-reducers)
* [Store](#store)
* [UI](#ui)
* [Dispatching Actions](#dispatching-actions)
* [Selectors](#selectors)
* [Middleware](#middleware)
* [Async Actions](#async-actions)
* [React Redux](#react-redux)
* [React Redux with Hooks](#react-redux-with-hooks)

## Data flow diagram
![Redux data flow diagram](https://github.com/tw-wong/learn-js/blob/master/screenshot/redux_data_flow_diagram.gif)


## State
```javascript
// Define an initial state value for the app
const initialState = {
  value: 0
}
```

## Reducer

```javascript
// Create a "reducer" function that determines what the new state
// should be when something happens in the app
function counterReducer(state = initialState, action) {
  // Reducers usually look at the type of action that happened
  // to decide how to update the state
  switch (action.type) {
    case 'counter/incremented':
      return { ...state, value: state.value + 1 }
    case 'counter/decremented':
      return { ...state, value: state.value - 1 }
    default:
      // If the reducer doesn't care about this action type,
      // return the existing state unchanged
      return state
  }
}
```

* It has 2 arguments, the current `state` and `action` object.

* Action objects (`action`) always have a `type` field, which is a string you provide that acts as a unique name for the action. 

* Action object can have other fields with additional information about what happened. By convention, we put information in a field called `payload. Ex: 

  ```javascript
  const addTodoAction = {
      type: 'todos/todoAdded',
      payload: 'Buy milk'
  }    
  ```

* Action type format: "domain/eventName". Ex: `action.type = 'counter/incremented'`.

  ## Reducer rules
  * Should only calculate the new state value based on the `state` and `action` arguments.

  * Not allowed to modify the existing state. Instead, they must make **immutable** update. You should make copies of the original values, and then you can mutate the copies.

  * Not do any asynchronous logic or other "side effects" (logging, saving a file, Ajax request, generating random numbers or unique random IDs) in `reducer` function. 

  ## Splitting Reducers

  If we have multiple section at `State`, we can split a large `Reducer` to smaller Reducer functions (slice reducer).

    * Before split:

      ```javascript        
      //state with multiple sections:
      const initialSate = {
        todos: [], 
        filters: [],
      }

      //File: src/reducer.js
      export default function appReducer(state = initialState, action) {
        switch (action.type) {
          //handle action belongs to "todo" state.
          case 'todos/todoAdded': {
            return { ... }
          }
          case 'todos/todoToggled': {
            return { ... }
          }

          //handle action belongs to "filter" state.
          case 'filters/statusFilterChanged': {
            return { ... }
          }
          case 'filters/statusFilterDeleted': {
            return { ... }
          }

          ...
        }
      }
      ```

    * After split:
        
      ```javascript
        //File: src/features/todos/todosSlice.js
      const initialState = [];

      export default function todosReducer(state = initialState, action) {
        switch (action.type) {
          case 'todos/todoAdded': {
            return { ... }
          }
          case 'todos/todoToggled': {
            return { ... }
          }

          ...
                          
          default:
            return state
        }
      }

      //File: src/features/filters/filterSlice.js
      const initialState = [];

      export default function filterReducer(state = initialState, action) {
        switch (action.type) {
          case 'filters/statusFilterChanged': {
            return { ... }
          }
          case 'filters/statusFilterDeleted': {
            return { ... }
          }

          ...
                          
          default:
            return state
        }
      }
      ```

  ## Combining Reducers

    ```javascript
    //File: src/reducer.js
    import { combineReducers } from 'redux'
    import todosReducer from './features/todos/todosSlice'
    import filtersReducer from './features/filters/filtersSlice'

    const rootReducer = combineReducers({
      // Define a top-level state field named `todos`, handled by `todosReducer`
      todos: todosReducer,
      filters: filtersReducer
    })

    export default rootReducer
    ```

    * The key names you give to `combineReducers` decides what the key names of your state object will be! Ex: `state.todos` will only be parsed to `todosReducer(state, action)` as first argument.


## Store

```javascript
// Create a new Redux store with the `createStore` function,
// and use the `counterReducer` for the update logic
const store = Redux.createStore(counterReducer)

// Or can create using @reduxjs/toolkit
import { configureStore } from '@reduxjs/toolkit'
const store = configureStore({ reducer: counterReducer })
```

## UI

```javascript
// Our "user interface" is some text in a single HTML element
const valueEl = document.getElementById('value')

// Whenever the store state changes, update the UI by
// reading the latest store state and showing new data
function render() {
  const state = store.getState()
  valueEl.innerHTML = state.value.toString()
}

// Update the UI with the initial data
render()
// And subscribe to redraw whenever the data changes in the future
store.subscribe(render)
```

* Get latest state from Redux store using `store.getState()`.

* `store.subscribe()` to pass a subscriber callback function that will be called every time the store is updated. 

## Dispatching Actions

```javascript
// Handle user inputs by "dispatching" action objects,
// which should describe "what happened" in the app
document.getElementById('increment').addEventListener('click', function () {
  store.dispatch({ type: 'counter/incremented' })
})

document.getElementById('decrement').addEventListener('click', function () {
  store.dispatch({ type: 'counter/decremented' })
})

document
  .getElementById('incrementIfOdd')
  .addEventListener('click', function () {
    // We can write logic to decide what to do based on the state
    if (store.getState().value % 2 !== 0) {
      store.dispatch({ type: 'counter/incremented' })
    }
  })

document
  .getElementById('incrementAsync')
  .addEventListener('click', function () {
    // We can also write async logic that interacts with the store
    setTimeout(function () {
      store.dispatch({ type: 'counter/incremented' })
    }, 1000)
  })
```
* `Action` is an event that describes something that happened in the application.

* When user click a button (event), create an `Action` that describes what happened, and `dispatch` them to the `store`.

* Call `store.dispatch(action)` and the `store` runs `reducer`, update the `state`, and runs the `subscribers` to update the UI.

## Selectors

```javascript
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
```

* `Selectors` are functions that know how to extract specific pieces of information from a store state value. 

* This can help avoid repeating logic as different parts of the app need to read the same data.

## Middleware
* A third-party extension point between `dispatching an action` and the moment is reaches `reducer`.

* Without middleware: `Dispatch Action` -> `Reducer`.
* With middleware: `Dispatch Action` -> Middleware (third-party) -> `Reducer`.

* Use middleware for logging (ex: redux-logger), crash reporting, performing asynchronous tasks (ex: redux-thunk) and etc.

    ```sh
    ~ npm install redux-logger
    ```
* Usage:

    ```javascript
    const redux = require('redux')
    const reduxLogger = require('redux-logger')

    const createStore = redux.createStore
    const combineReducers = redux.combineReducers
    const applyMiddleware = redux.applyMiddleware
    const logger = reduxLogger.createLogger()

    ...

    const rootReducer = combineReducers({
      aaa: aaaReducer, 
      bbb: bbbReducer, 
    })
    const store = createStore(rootReducer, applyMiddleware(logger))
    ```

## Async Actions

* Use case: Async API call to fetch data from an end point and use that data in the application.

* To create async action creators, we need third-party extensions:
    * `axios`: 
      * Request to an API endpoint.

    * `redux-thunk`: 
      * Define async action creators.
      * Act as middleware.

    * Installation:
        
      ```sh
      ~ npm install axios redux-thunk
      ```

```javascript

const applyMiddleware = redux.applyMiddleware
const thunkMiddleware = require('redux-thunk').default
const axios = require('axios')

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case FETCH_USER_REQUEST:
      return { 
        ...state, 
        loading: true
      }

    case FETCH_USER_SUCCESS:
      return { 
        loading: false,
        users: action.payload, 
        error: ''
      }

    case FETCH_USER_FAILURE:
      return { 
        loading: false,
        users: [], 
        error: action.payload
      }

    default:
      return state
  }
}

// Define initial state
const initialState = {
  loading: false, 
  users: [], 
  error: ''
}

// Define actions
const fetchUsersRequest = () => {
  return {
    type: FETCH_USER_REQUEST
  }
}

const fetchUsersSuccess = users => {
  return {
    type: FETCH_USER_SUCCESS, 
    payload: users
  }
}

const fetchUsersFailure = error => {
  return {
    type: FETCH_USER_FAILURE, 
    payload: error
  }
}

// Async action creators, return as a function.
const fetchUsers = () => {
  return function(dispatch) {

    //dispatch action: set loading to true before async request.
    dispatch(fetchUsersRequest())

    axios.get('https://jsonplaceholder.typicode.com/users')
    .then(response => {
      //response.data is the array of users.

      //only return user id
      const users = response.data.map(user => user.id)

      //dispatch action: update users state data.
      dispatch(fetchUsersSuccess(users))
    })
    .catch(error => {
      //error.message is the error description.

      //dispatch action: failed to fetch users data
      dispatch(fetchUsersFailure(error.message))
    })
  }
}

const store = createStore(rootReducer, applyMiddleware(thunkMiddleware))

//for debugging purpose: output the state
const store.subscribe(() => {
  console.log(store.getState())
})

//trigger the fetch user async action
const store.dispatch(fetchUsers)
```

## React Redux
// TODO
## React Redux with Hooks
// TODO

Refs:
* https://redux.js.org/tutorials/fundamentals/part-1-overview
* https://redux.js.org/tutorials/fundamentals/part-2-concepts-data-flow

