---
layout: post
title: Learning Javascript
---

This is going to be another rather lengthy note documenting my learning as I'm going through the React course on Scrimba. Here we go:

### First React code, why React, and building a React info site
---

#### Setting up
- This is mostly going through an easy setup and starting a simple React project.

  To start, adding the CDN links from [here](https://reactjs.org/docs/cdn-links.html) script to the `<head>` tag of an HTML document:
  
  ```HTML
  <script crossorigin src="https://unpkg.com/react@17/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
  ```
  
  and another `<script>` to use Babel transformer under `<body>` tag:
  
  ```HTML
  <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
  ```

  To print something on screen, use `ReactDOM.render(what_to_print, where_to_print_it)`, for example:
  
  ```javascript
  ReactDOM.render(<h1>Hello, everyone!</h1>, document.getElementById("root"))
  ```
  
  this will print out "Hello, everyone!" in `<h1>` style.

- A more correct way to set up React would be adding these two lines in .js file that we're working with:
- 
  ```javascript
  import React from "react"
  import ReactDOM from "react-dom"
  ```
  
- Set up React locally with Create React App:
  - First, make sure that we have Node.js and NPM installed on local machine (type into Terminal: `node -v` and `npm -v` to check. If we don't already have Node.js, we need to install it with [nvm](https://github.com/nvm-sh/nvm) or [nvm-window](https://github.com/coreybutler/nvm-windows)
  - Go to [Create React App](https://create-react-app.dev/) webpage
  - To create a project called _my-app_, run this command in Terminal:
 
    ```
    npx create-react-app my-app
    ```
    
  - Then run:
  
    ```
    cd my-app
    npm start
    ```
    
  - If run correctly, we should see something like `Compiled successfully!` and default React screen

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

  function MainPage() {
    return (
        <div>
            <header>
                <nav>
                    <img src="./react-logo.png" width="40px" />
                </nav>
            </header>
            <h1>Reasons I'm excited to learn React</h1>
            <ol>
                <li>It's a popular library, so I'll be 
                able to fit in with the cool kids!</li>
                <li>I'm more likely to get a job as a developer
                if I know React</li>
            </ol>
            <footer>
                <small>© 2021 Ziroll development. All rights reserved.</small>
            </footer>
        </div>
    )
  }

  ReactDOM.render(<MainPage />, document.getElementById("root"))
  ```
- We need to use PascalCase instead of camelCase to name React component.
- Now, instead of keeping everythinng in one parent `MainPage` component, we can separate the above code out to different child components and render them:
  ```javascript
  import React from "react"
  import ReactDOM from "react-dom"
  
  function Header() {
    return (
        <header>
            <nav>
                <img src="./react-logo.png" width="40px" />
            </nav>
        </header>
    )
  }

  function Footer() {
      return (
          <footer>
              <small>© 2021 Ziroll development. All rights reserved.</small>
          </footer>
      )
  }

  function MainContent() {
      return (
          <div>
              <h1>Reasons I'm excited to learn React</h1>
              <ol>
                  <li>It's a popular library, so I'll be 
                  able to fit in with the cool kids!</li>
                  <li>I'm more likely to get a job as a developer
                  if I know React</li>
              </ol>
          </div>
      )
  }

  function Page() {
      return (
          <div>
              <Header />
              <MainContent />
              <Footer />
          </div>
      )
  }

  ReactDOM.render(<Page />, document.getElementById("root"))
  ```
- Styling in React is very similar to styling in HTML and CSS, but insead of `class=""` we use `className=""`

#### Organizing components
- Best practice is to separate a substantial React component out to its own `.js` file. For example, we can create a `Header.js` file to contain the `Header` component above. In this file, there are a few things that we need to do to make this component works:
  As usual, add:
  ```javascript
  import React from "react"
  ```
  Then add `export default` like this to the function name:
  ```javascript
  export default function Header() {}
  ```
  Over at the `index.js` (or any other `.js` file that we will need to use this `Header` component), add:
  ```javascript
  import Header from "./Header"
  ```
- For styling, we will put a `style.css` (or other `.css` files that we will use to style our webpage) in the `src` folder. Then to import this style file:
  ```javascript
  import "./style.css"
  ```
- For images, we'll put them in `images` folder within `src` folder. Then import them to the `.js` file that renders our page, for example:
  ```javascript
  import reactLogo from "../images/react-icon-small.png"
  ```
  and then instead of using the relative path within `<img>` tag, we just refer to the image name:
  ```javascript
  <img
    src={reactLogo}
    alt="React Logo"
    className="nav--icon"
  />
  ```
- Otherwise, we can use the Public folder to keep these images, but generally, there are some negatives to that.
- `::marker` is a CSS pseudo-element that selects markers of a bulletted or numbered list. Read more [here](https://developer.mozilla.org/en-US/docs/Web/CSS/::marker)

### Data-driven React
---

#### Props - Reusable Components
- Putting variable name inside `{}` to inteprete it (similar to `${}` in JS). Furthermote, anything in between `{}` will be run as javascript.
- Props (or attributes) in React makes a component for reusable. Props are JS Objects that are passed into a component in order for it to work correctly, similar to how a function receives parameters. Props are immutable, meaning a component receiving props is not allowed to modify these props. Here is a simple example of how props are used:
  ```javascript
  import React from "react"
  
  export default function Contact(props) {
    return (
        <div className="contact-card">
            <img src={props.img}/>
            <h3>{props.name}</h3>
            <div className="info-group">
                <img src="./images/phone-icon.png" />
                <p>{props.phone}</p>
            </div>
            <div className="info-group">
                <img src="./images/mail-icon.png" />
                <p>{props.email}</p>
            </div>
        </div>
    )
  }
  ```
  where `props.img`, `props.name`, `props.phone`, `props.email` are props that are going to be rendered. WE can also destructure this props object and write 
  ```react
  export default function Contact({img, name, phone, email}) {
  ...
  }
  ```
- By default, props are string, so we use `{}` to contain other types of data other than string, such as `price={150}`
- A simple Jokes project to illustrate how to use props and `.maps()` method (which converts an array of raw data into an array of JSX elements that can be displayed on the page and makes our code more self-sustaining, not requiring changes when new data with the same structured are being generated and needed to be displayed):
  - We have a `jokesData.js` to contain all jokes, each with a `setup` and `punchline`:
  
    ```javascript
    export default [
    {
        setup: "I got my daughter a fridge for her birthday.",
        punchline: "I can't wait to see her face light up when she opens it."
    },
    {
        setup: "How did the hacker escape the police?",
        punchline: "He just ransomware!"
    },
    {
        setup: "Why don't pirates travel on mountain roads?",
        punchline: "Scurvy."
    },
    {
        setup: "Why do bees stay in the hive in the winter?",
        punchline: "Swarm."
    },
    {
        setup: "What's the best thing about Switzerland?",
        punchline: "I don't know, but the flag is a big plus!"
    }
    ]
    ```
  - Now, we have a `Joke.js` file which is a React component that will tell how jokes are displayed on a page:
    ```javascript
    import React from "react"

    export default function Joke(props) {
      return (
        <div>
            {props.setup && <h3>Setup: {props.setup}</h3>}
            <p>Punchline: {props.punchline}</p>
            <hr />
        </div>
      ) 
    }
    ```
    
  - Lastly, we'll compose an `App.js` file that will take in `Joke.js` component (and ideally other components as well, in the case of a more realistic webpage) and display:
  
    ```javascript
    import React from "react"
    import Joke from "./Joke"
    import jokesData from "./jokesData"

    export default function App() {
      const jokeElements = jokesData.map(joke => {
        return <Joke setup={joke.setup} punchline={joke.punchline} />
      })
      return (
          <div>
              {jokeElements}
          </div>
      )
    }
    ```
    
- We can actually make some small changes to `Joke.js` and `App.js` above to make our code more concise:
  In `Joke.js`, instead of `props.setup`, we'll use: `props.item.setup`:
  
  ```javascript
  import React from "react"

  export default function Joke(props) {
    return (
      <div>
        {props.item.setup && <h3>Setup: {props.item.setup}</h3>}
          <p>Punchline: {props.item.punchline}</p>
          <hr />
      </div>
     ) 
  }
  ```
  
  that will allow us to simplify `App.js`:
  
  ```javascript
  import React from "react"
  import Joke from "./Joke"
  import jokesData from "./jokesData"

  export default function App() {
    const jokeElements = jokesData.map(joke => {
        return <Joke item={item} />
      })
      return (
          <div>
              {jokeElements}
          </div>
      )
    }
  ```
  this might not look like a lot of improvements here, but for a page with more props/attributes to display, `item={item}` will really make a difference.
- Another convention tha is commonly used is to spread object as props. So for the example above, we'll keep `props.setup` and `props.punchline` in `Jokes.js` file and in `App.js`, instead of `item={item}`, we'll use the spreading syntax `{...item}`. However, this might be a little bit confusing sometimes.

### Building interactive web apps with React
---

#### Event Listeners
- `onClick` is a event handler/listener that will do something when an element (typically a button) is clicked, for example, in this code below, we have console prints out "I was clicked!" when a button is clicked on (notice that we write `{handleClick}` instead of `{handleClick()}` because in the second way, this function will run as soon as the page is rendered.
  
  ```javascript
  export default function App() {
    function handleClick() {
        console.log("I was clicked!")
    }
    return (
        <div className="container">
            <img src="https://picsum.photos/640/360" />
            <button onClick={handleClick}>Click me</button>
        </div>
    )
  }
  ```
- A list of all events in React can be found [here](https://reactjs.org/docs/events.html). Most of the time, we'll be using mouse events.

#### State
- State is different from Props (see above) in that State refers to values that are managed by the component (hence it's expected to be changed by the component), similar to variables declared inside a function. Any time we have changing values that should be saved/displayed, we'll likely be using state. 
- We either type `React.useState()` everytime we need to use it once we've already imported React, or we can do `import { useState } from 'react'` and skip the `React.` part
- `React.useState()` is a React method/hook that returns an array `[some_value, f()]`. A `useState()` composes of a state variable or current state and a function to update the state variable. We also need to pass in an initial state (such as count = 0 or "active", etc.):
  
  ```javascript
  [count, setCount] = useState(0)
  ```
  
  In this case, `count` is the state variable, `setCount` is the function that update `count` and `0` is the initial count.
- If we ever need the old value of state to help you determine the new value of state, we should pass a callback function to our state setter function (`setCount` above) instead of using state directly. This callback function will receive the old value of state as its parameter, which we can then use to determine our new value of state. For example, instead of:
  
  ```javascript
  function subtract() {
    setCount(count - 1)
  }
  ```
  
  where we change/update `count` directly, a more conventional way to do the same thing would be:
  
  ```javascript
  function subtract() {
    setCount(prevCount => prevCount - 1)
  }
  ```
  
- Additional documentation on State and how to use is can be found [here](https://reactjs.org/docs/hooks-state.html)








