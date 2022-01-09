# Learning React

## What is React
- React is a JavaScript library, used to build user interfaces (UI) on the front end.
    - React is not a framework (unlike Angular, which is more opinionated).
    - open-source project created by Facebook.
    - React is just some JavaScript helper libraries that we can load into our HTML.
- React is the view layer of an MVC application (Model View Controller)

- One of the most important aspects of React is the fact that you can create **components**, which are like custom, reusable HTML elements, to quickly and efficiently build user interfaces. React also streamlines how data is stored and handled, using **state** and **props**.

## Create React app
-  [Create React App](https://github.com/facebook/create-react-app) is an environment that comes pre-configured with everything you need to build a React app.
    ```bash
    $ npx create-react-app react-tutorial
    $ cd react-tutorial && npm start
    ```

- The `/src` directory will contain all React code.
- For `index.css`, if you want, you can use Bootstrap or whatever CSS framework you want, or nothing at all.

- We use `className` instead of `class`. This is our first hint that the code being written here is JavaScript, and not actually HTML.

    ```{admonition} `App.js`
    ``` <!-- This time, we are loading the Component as a property of React, so we no longer need to extend React.Component. -->

    import React, {Component} from 'react'
    import ReactDOM from 'react-dom'
    import './index.css'

    class App extends Component {
    render() {
        return (
        <div className="App">
            <h1>Hello, React!</h1>
        </div>
        )
    }
    }

    ReactDOM.render(<App />, document.getElementById('root'))
    ```
    ```


## React Developer Tools
- There is an extension called React Developer Tools that will make your life much easier when working with React. Download [React DevTools for Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi), or whatever browser you prefer to work on.


## JSX: JavaScript + XML
- we've been using what looks like HTML in our React code, but it's not quite HTML. This is JSX, which stands for JavaScript XML.
    - With JSX, we can write what looks like HTML, and also we can create and use our own XML-like tags.

- Using JSX is not mandatory for writing React. Under the hood, it's running `createElement`, which takes the tag, object containing the properties, and children of the component and renders the same information.
    ```JS
    <!-- JSX -->
    const heading = <h1 className="site-heading">Hello, React</h1>

    <!-- No JSX -->
    const heading = React.createElement('h1', {className: 'site-heading'}, 'Hello, React!')
    ```

- JSX is actually closer to JavaScript, not HTML, so there are a few key differences to note when writing it.
    1. `className` is used instead of `class` for adding CSS classes, as `class` is a reserved keyword in JavaScript.
    2. Properties and methods in JSX are camelCase - `onclick` will become `onClick`.
    3. Self-closing tags must end in a slash - e.g.` <img />`

- JavaScript expressions can also be embedded inside JSX using curly braces, including variables, functions, and properties.
    ```js
    const name = 'Tania'
    const heading = <h1>Hello, {name}</h1>
    ```

## Components
- Almost everything in React consists of components, which can be **class components** or **simple components**.
    - 即可以导入到另一个文件的`.js`文件模组





## References
1. [React Tutorial: An Overview and Walkthrough](https://www.taniarascia.com/getting-started-with-react/)