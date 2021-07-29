# React context

## Overview
* [Concept](#concept)

* [Context and Provider](#context-and-provider)

* [Accessing Context](#accessing-context)

* [Updating Context data](#updating-context-data)

* [Multiple Context](#multiple-context)

## Concept

* Share state within a component tree (ex: UI theme setting, authenticated user info, or preferred language).

* Avoid passing props through all children component, although the state data is not required for intermediate component.

## Context and Provider

```javascript
// File: src/contexts/ThemeContext.js

import React, { createContext, Component } from 'react'

//Create a context object
export const ThemeContext = createContext()

class ThemeContextProvider extends Component {
  state = {
    isLightTheme: true,
    light: {bg: '#eee'},
    dark: {bg: '#555'},
  }

  render() {
    return (
      <ThemeContext.Provider value={...this.state}>
        {this.props.children}
      </ThemeContext.Provider>
    )
  }
}

export default ThemeContextProvider


//File: src/App.js
...

function App() {
  return (
    <div className="App">
      <ThemeContextProvider>
        <Navbar />
        <BookList />
      </ThemeContextProvider>
    </div>
  )
}
```

* Create a Context object (`ThemeContext`).

* Create a Provider React component (`ThemeContextProvider`) that allows consuming components to subscribe to context changes.

* One Provider (`ThemeContextProvider`) can be connected to many consumers (children component).

* Provider component (`ThemeContextProvider`) accepts a `value` prop to be passed to consuming components that are descendants of this Provider.

* All consumers that are descendants of a Provider will re-render whenever the Provider’s value prop changes.

* In `src/App.js`, wrap the Provider component (`ThemeContextProvider`) to children components (`Navbar`, `BookList`).

## Accessing Context

```javascript
//File: src/components/NavBar.js
import React, { Component } from 'react'
import { ThemeContext } from '../contexts/ThemeContext'

class Navbar extends Component {
  // context to consume in this component
  static contextType = ThemeContext

  return (
    const {isLightTheme, light, dark} = this.context
    const theme = isLightTheme ? light : dark

    <nav style={{ background: theme.bg }}>
      { /* ... */ }
    </nav>
  )
}

export default NavList
```

* `contextType` property on a class can be assigned a Context object created by `React.createContext()` (`ThemeContext`).

* This method only suitable to subscribe a single context in the component.


## Updating Context data

// TODO

## Multiple Context

// TODO