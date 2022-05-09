---
layout: post
title: Learning Javascript
---

This is going to be another rather lengthy note documenting my learning as I'm going through the React course on Scrimba. Here we go:

### First React code, wHy React, and building a React info site
---
#### Setting up
- This is mostly going through an easy setup and starting a simple React project.

  To start, adding the CDN links from [here](https://reactjs.org/docs/cdn-links.html) script to the `<head>` tag of an HTML document:
  ```HTML
  <script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
  ```
  and another `<script>` to use Babel transformer (from [here](<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>))under `<body>` tag:
  ```HTML
  <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
  ```

  To print something on screen, use `ReactDOM.render(what_to_print, where_to_print_it)`, for example:
  ```javascript
  ReactDOM.render(<h1>Hello, everyone!</h1>, document.getElementById("root"))
  ```
  this will print out "Hello, everyone!" in `<h1>` style.

- A more correct way to set up React would be adding these two lines in .js file that we're working with:
  ```javascript
  import React from "react"
  import ReactDOM from "react-dom"
  ```

#### Creating first React component
**React is composable**: Creating React component using JS function then `ReactDOM.render()` it. For example:
```javascript
function MainContent() {
    return (
        <h1>I'm learning React!</h1>
    )
}

ReactDOM.render(
    <div>
      <MainContent />
    </div>,
    document.getElementById("root")
)
```
**React is declarative**: 
_Declarative_ = What should be done? vs. _Imperative_ = How should it be done?
An imperative way to write our previous React code
  ```javascript
  ReactDOM.render(<h1>Hello, everyone!</h1>, document.getElementById("root"))
  ```
  is to write these in JS:
  ```javascript
  const h1 = document.createElement("h1")
  h1.textContent = "This is an imperative way to program"
  h1.className = "header"
  document.getElementById("root").append(h1)
  ```
  which is a lot more complicated and time-consuming. 

#### JSX
- JSX makes React declarative rather than imperative by creating JS Objects.
- Only render one parent element at a time, for example:
- Rather than doing this and render both `<h1>` and `<p>` elements:
  ```javascript
  ReactDOM.render(
    <h1 className="header">This is JSX</h1>
    <p>This is a paragraph</p>,
    document.getElementById("root")
  )
  ```
  do this and wrap both of them inside a parent `<div>` tag instead:
  ```javascript
  ReactDOM.render(
    <div>
      <h1 className="header">This is JSX</h1>
      <p>This is a paragraph</p>
    </div>,
    document.getElementById("root")
  )
  ```
- We can also save the whole `<div>` tag above under a variable.

#### Custom components
- Reusable components in React are JS functions that we can call to return React elements. For a simple webpage, this is how it will look like:
  ```javascript
  import React from "react"
  import ReactDOM from "react-dom"

  function TemporaryName() {
      return (
          <div>
              <img src="./react-logo.png" width="40px" />
              <h1>Fun facts about React</h1>
              <ul>
                  <li>Was first released in 2013</li>
                  <li>Was originally created by Jordan Walke</li>
                  <li>Has well over 100K stars on GitHub</li>
                  <li>Is maintained by Facebook</li>
                  <li>Powers thousands of enterprise apps, including mobile apps</li>
              </ul>
          </div>
      )
  }

  ReactDOM.render(<TemporaryName />, document.getElementById("root"))
  ```
- We need to use PascalCase instead of camelCase to name React component.
- 





