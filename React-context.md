# React context

## Overview
* [Concept](#concept)

* [Context and Provider](#context-and-provider)

* [Accessing Context](#accessing-context)

* [Updating Context data](#updating-context-data)

* [Multiple Context](#multiple-context)

* [Multiple Contexts with Hooks](#multiple-contexts-with-hooks)

* [Creating Context with functional component](#creating-context-with-functional-component)

## Concept

* Share state within a component tree (ex: UI theme setting, authenticated user info, or preferred language).

* Avoid passing props through all children component, although the state data is not required for intermediate component.

## Context and Provider

```javascript
// File: src/contexts/ThemeContext.js
import React, { createContext, Component } from 'react'

// Create a context object
export const ThemeContext = createContext()

class ThemeContextProvider extends Component {
  state = {
    isLightTheme: true,
    light: {bg: '#eee'},
    dark: {bg: '#555'},
  }

  render() {
    return (
      <ThemeContext.Provider value={{ ...this.state }}>
        {this.props.children}
      </ThemeContext.Provider>
    )
  }
}

export default ThemeContextProvider


// File: src/App.js
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

* Create a Provider (`ThemeContextProvider`) that allows consuming components to subscribe to context changes.

* One Provider (`ThemeContextProvider`) can be connected to many consumers (children component).

* Provider component (`ThemeContextProvider`) accepts a `value` prop to be passed to consuming components that are descendants of this Provider.

* All consumers that are descendants of a Provider will re-render whenever the Provider’s value prop changes.

* In `src/App.js`, wrap the Provider component (`ThemeContextProvider`) to children components (`Navbar`, `BookList`).

## Accessing Context

```javascript
// File: src/components/NavBar.js
import React, { Component } from 'react'
import { ThemeContext } from '../contexts/ThemeContext'

class Navbar extends Component {
  // Context to consume in this component
  static contextType = ThemeContext

  return (
    const { isLightTheme, light, dark } = this.context
    const theme = isLightTheme ? light : dark

    <nav style={{ background: theme.bg }}>
      { /* ... */ }
    </nav>
  )
}

export default NavList
```

* `contextType` property on a class can be assigned a Context object created by `React.createContext()` (`ThemeContext`).

* This method only suitable to subscribe a **single** context in the component.


## Updating Context data

```javascript
// File: src/contexts/ThemeContext.js
import React, { createContext, Component } from 'react'

// Create a context object
export const ThemeContext = createContext()

class ThemeContextProvider extends Component {
  state = {
    isLightTheme: true,
    light: {bg: '#eee'},
    dark: {bg: '#555'},
  }

  // Create toggle function to update the state
  toggleTheme = () => {
    this.setState((prevState, props) => {
      return {isLightTheme: !prevState.isLightTheme}
    });
  }

  render() {
    return (
      // Pass in toggle function, so that the consumers (children components) can use it.
      <ThemeContext.Provider value={{ ...this.state, toggleTheme: this.toggleTheme }}>
        {this.props.children}
      </ThemeContext.Provider>
    )
  }
}

export default ThemeContextProvider

//File: src/components/ThemeToggle.js
class ThemeToggle extends Component {
  static contextType = ThemeContext

  render() {
    // refer toggle function from context provider (ThemeContext)
    const { toggleTheme } = this.context

    <button onClick={ toggleTheme }>Toggle the theme</button>
  }
}

export default ThemeToggle

// File: App.js
function App() {
  return (
    <div className="App">
      <ThemeContextProvider>
        <Navbar />
        <BookList />

        {/*  Add toggle component */}
        <ThemeToggle />
      </ThemeContextProvider>
    </div>
  )
}
```

* Create update Context function (`toggleTheme`) at Context Provider (`ThemeContextProvider`), make it accessible from its children components (`ThemeToggle`).

* When consuming context in child component (`ThemeToggle`), refer the update Context function (`toggleThem`) from the Context  (`ThemeContext`), trigger the function when button is clicked.

## Multiple Context

```javascript

// Create 2 contexts:
// File: src/contexts/ThemeContext.js
// File: src/contexts/AuthContext.js

// File: App.js
function App() {
  return (
    <div className="App">
      {/* wrap provider for ThemeContext*/}
      <ThemeContextProvider>
        {/* wrap provider for AuthContext*/}
        <AuthContextProvider>
          <Navbar />
          <BookList />
          <ThemeToggle />
        </AuthContextProvider>
      </ThemeContextProvider>
    </div>
  )
}

// File: src/components/NavBar.js
import React, { Component } from 'react'
import { ThemeContext } from '../contexts/ThemeContext'
import { AuthContext } from '../contexts/AuthContext'

class Navbar extends Component {
  return (
    { /* Use context.Consumer when using multiple contexts. */ }
    <AuthContext.Consumer>{(authContext) => {
      <ThemeContext.Consumer>{(themeContext) => {

        const { isAuthenticated, toggleAuth } = authContext
        const { isLightTheme, light, dark } = themeContext
        const theme = isLightTheme ? light : dark

        <div onClick={ toggleAuth }>
          { isAuthenticated ? 'Logged in' : 'Logged out'}
        </div>
        <nav style={{ background: theme.bg }}>
          { /* ... */ }
        </nav>
      }}</themeContext.Consumer>
    }}</AuthContext.Consumer>
  )
}

export default NavList
```

* Create multiple Contexts (`ThemeContext, AuthContext`) and wrapping it (`ThemeContextProvider, AuthContextProvider`) to its children components.

* Use `Context.Consumer` in child component when consuming multiple contexts.


## Multiple Contexts with Hooks

```javascript
// File: src/components/NavBar.js
import React, { useContext } from 'react'
import { ThemeContext } from '../contexts/ThemeContext'
import { AuthContext } from '../contexts/AuthContext'

const NavBar = () => {

  // use hook: useContext to referring context.
  const { isAuthenticated, toggleAuth } = useContext(AuthContext)
  const { isLightTheme, light, dark } = useContext(ThemeContext)
  const theme = isLightTheme ? light : dark

  return (
    <div onClick={ toggleAuth }>
      { isAuthenticated ? 'Logged in' : 'Logged out'}
    </div>
    <nav style={{ background: theme.bg }}>
      { /* ... */ }
    </nav>
  )
}
```

* Use hook `useContext` when referring a context in functional component.

## Creating Context with functional component

```javascript
// File: src/context/BookContext.js
import React, { createContext, useState} from 'react'

export const BookContext = createContext()

// Create context Provider
const BookContextProvider = (props) => {
  const defaultBooks = [
    {id: 1, title: 'Book a'},
    {id: 2, title: 'Book b'},
    {id: 3, title: 'Book c'},
  ]

  const [books, setBooks] = useState(defaultBooks)

  return (
    <BookContext.Provider value={books}>
      { props.children }
    </BookContext.Provider>
  )
}

export default BookContextProvider

// File: App.js
function App() {
  return (
    <div className="App">
      <ThemeContextProvider>
        <AuthContextProvider>
          <Navbar />

          {/* wrap provider for BookContext*/}
          <BookContextProvider>
            <BookList />
          </BookContextProvider>

          <ThemeToggle />
        </AuthContextProvider>
      </ThemeContextProvider>
    </div>
  )
}

// File: src/components/BookList.js
import React, { useContext } from 'react'
import { ThemeContext } from '../contexts/ThemeContext'
import { AuthContext } from '../contexts/AuthContext'
import { BookContext } from '../contexts/BookContext'

const BookList = () => {
  const { isAuthenticated, toggleAuth } = useContext(AuthContext)
  const { isLightTheme, light, dark } = useContext(ThemeContext)
  const theme = isLightTheme ? light : dark

  // refer BookContext written in functional component.
  const { book } = useContext(BookContext)

  return (
    <div onClick={ toggleAuth }>
      { isAuthenticated ? 'Logged in' : 'Logged out'}
    </div>
    <nav style={{ background: theme.bg }}>
      { /* ... */ }
    </nav>


    <div style={{ background: theme.bg }}>
      <ul>
        {books.map(book => {
          return (
            <li key={ book.id }>{ book.title }</li>
          )
        })}
      </ul>
    </div>
  )
}
```