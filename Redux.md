# Redux

## Overview
* [Data flow diagram](#data-flow-diagram)
* [State](#state)
* [Reducer](#reducer)
* [Store](#store)
* [UI](#ui)
* [Dispatching Actions](#dispatching-actions)
* [Selectors](#selectors)

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


Refs:
* https://redux.js.org/tutorials/fundamentals/part-1-overview
* https://redux.js.org/tutorials/fundamentals/part-2-concepts-data-flow

