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

    ```java
     <!-- This time, we are loading the Component as a property of React, so we no longer need to extend React.Component. -->

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

- Syntax for React functions
    ```java
    <!-- if not within a class need to include const -->
    const function_name = (paras) => {
        code
    }
    ```

## React Developer Tools
- There is an extension called React Developer Tools that will make your life much easier when working with React. Download [React DevTools for Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi), or whatever browser you prefer to work on.


## JSX: JavaScript + XML
- it is what looks like HTML in React code, but it's not quite HTML. This is JSX, which stands for JavaScript XML.
    - With JSX, we can write what looks like HTML, and also we can create and use our own XML-like tags.

- Using JSX is not mandatory for writing React. Under the hood, it's running `createElement`, which takes the tag, object containing the properties, and children of the component and renders the same information.
    ```java
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
    ```java
    const name = 'Tania'
    const heading = <h1>Hello, {name}</h1>
    ```

## Elements
TO ADD.


## Components
- Almost everything in React consists of components, which can be **class components** or **simple components**.
    - component可以直接写在最终文档里或者也可以写成一个单独的文件，即可以导入到另一个文件的`.js`文件模组; 可以在各处重复使用。
- React components can be declared either as JS **functions** or ES6 **class**
- Coverting an function component to a class in five steps:
    1. Create an ES6 class, with the same name, that extends `React.Component`
    2. Add a signle empty method to it called `render()`
    3. Move the body of the function into the `render()` method
    4. Replace `props` with `this.props` in the `render()` body
    5. Delete the remaining empty function declaration
    ```javascript
    class Clock extends React.Component {
        render() {
            return (
                <div>
                    <h1>Hello, world!</h1>
                    <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
                </div>
            );
        }
    }
    ```

- A good rule of thumb is that if a part of your UI is used several times (Button, Panel, Avatar), or is complex enough on its own (App, FeedStory, Comment), it is a good candidate to be extracted to a separate component.

### Class Components
- We create a custom class component as we wanted
    - We capitalize custom components to differentiate them from regular HTML elements.
- Import the component in the e.g. `App.js` file
- Then by loading it into the `render()` of `App`, we load the component
    
    ```
    class ClassComponent extends Component {
        render() {
            return <div>Example</div>
        }
    }
    export default ClassComponent
    ```

### Simple Components
- Simple component is a function. This component doesn't use the `class` keyword.
    ```
    const SimpleComponent = () => {
        return <div>Example</div>
    }
    ```
- Simple components can be used withwin class components
- components can be nested in other components, and simple and class components can be mixed.

    ```{note}
    A class component must include `render()`, and the `return` can only return one parent element.
    ```

### Props
- One of the big deals about React is how it handles data, and it does so with properties, referred to as **props**, and with state.
-  we can access all props through `this.props`
- You should always use **keys** when making lists in React, as they help identify each list item.
- Props are an effective way to pass existing data to a React component, however the component cannot change the props - they're read-only. 
- We (React offical doc) recommend naming props from the component’s own point of view rather than the context in which it is being used.
- Props are Read-Only; **All React components must act like pure functions with respect to their props**.

    ```javascript
    const TableBody = (props) => {
        const rows = props.characterData.map((row, index) => {
            return (
                <tr key={index}>
                    <td>{row.name}</td>
                    <td>{row.job}</td>
                </tr>
            )
        })

        return <tbody>{rows}</tbody>
    }

    class Table extends Component {
        render() {
            const {characterData} = this.props

            return (
            <table>
                <TableHeader />
                <TableBody characterData={characterData} />
            </table>
            )
        }
    }

    class App extends Component {
        render() {
            const characters = [
                {
                    name: 'Charlie',
                    job: 'Janitor',
                },
                {
                    name: 'Mac',
                    job: 'Bouncer',
                },
                {
                    name: 'Dee',
                    job: 'Aspring actress',
                },
                {
                    name: 'Dennis',
                    job: 'Bartender',
                }
            ]

            return (
                <div className="container">
                    <Table />
                </div>
            )
        }
    }
    ```

## State
- With props, we have a one way data flow, but with state we can update private data from a component.
- You can think of state as any data that should be saved and modified without necessarily being added to a database - for example, adding and removing items from a shopping cart before confirming your purchase.

- The object will contain properties for everything you want to store in the state.
    ```java
    class App extends Component {
        state = {
            characters: [
                {
                    name: 'Charlie',
                    // the rest of the data
                },
            ],
        }   
    }
    ```
- To retrieve the state, we'll get `this.state.characters` using the same ES6 method as before. To update the state, we'll use `this.setState()`, a built-in method for manipulating state. We'll filter the array based on an `index` that we pass through, and return the new array.

    ```{note}
    You must use this.setState() to modify an array. Simply applying a new value to this.state.property will not work.
    ```

- state can't be modified directly, instead, use `setState()`; The only place where you can assign `this.state` is the constructor.
- when update state, don't use `this.state` or `this.props` as input since they may be updated asynchronously (?)
- When calling `setState()`, React merges the object you provide into the current state.
- a state created with multiple variables that can be updated individually and independently with the final state merges the latest updates from all variables
- Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.
    - This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.
    - The value of the state in the parent component can be passed to the child component as props, but the child component wouldn't know whether it is state or props from the parent; This is commonly called a “top-down” or “unidirectional” data flow. 




## Using React Router for a SPA (single page application)
- **Routing** is how a web applications direct traffic.
- In Single Page Application (or SPA) - only one page is loaded, and every click to a new page loads some additional JSON data, but does not actually request a new resource like loading index.html and about-me.html would.

- Two potential dependencies for using routing
    - `$ npm install react-router-dom axios`

### Browser router
- To use `react-router-dom`, we need to wrap our entire `App` component in `BrowserRouter`. There are two types of routers:

    - `BrowserRouter` - makes pretty URLs like `example.com/about`.
    - `HashRouter` - makes URLs with the octothorpe (or hashtag, if you will) that would look like `example.com/#about`.


### Route and switch
- `Switch` - Groups all your routes together, and ensures they take precedence from top-to-bottom.
- `Route` - Each individual route.

### Link
- In order to link to a page **within** the SPA, we'll use `Link`. If we used the traditional `<a href="/route">`, it would make a completely new request and reload the page, so we have `Link` to help us out.


## Handling events
- Difference in handling events b/t React and HTML
    - Syntax difference
    - the way to prevent default behavior, i.e. in React, use `preventDefault()`

- Binding in JSX callbacks (?)
    ```javascript
    this.handleClick = this.handleClick.bind(this)
    ```

## Conditional Rendering
- Use `if` statement to conditionally render a component
- Use inline condtions in JSX
    - Inline `if` with logical `&&` operator: It works because in JavaScript, `true && expression` always evaluates to `expression`, and `false && expression` always evaluates to `false`.
    - Inline If-Else with Conditional Operator
        - avaScript conditional operator `condition ? true : false`.

- Use conditional rendering to prevent component from rendering
    - essentially return `null` when not wanting the component to be rendered
    - Returning `null` from a component’s `render` method does not affect the firing of the component’s lifecycle methods. For instance `componentDidUpdate` will still be called.

## Lists and kyes

```{margin}
Here is an [in-depth explanation about why keys are necessary(https://reactjs.org/docs/reconciliation.html#recursing-on-children)] if you’re interested in learning more.
```
- A “key” is a special string attribute you need to include when creating lists of elements.
- Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity
- The best way to pick a key is to use a string that uniquely identifies a list item among its siblings. Most often you would use IDs from your data as keys
    ```javascript
    const todoItems = todos.map((todo) =>
        <li key={todo.id}>
            {todo.text}
        </li>
        );
    ```
- When you don’t have stable IDs for rendered items, you may use the item index as a key as a last resort:
    ```javascript
    const todoItems = todos.map((todo, index) =>
        // Only do this if items have no stable IDs
        <li key={index}>
            {todo.text}
        </li>
    );
    ```

    ```{margin}
    Check out Robin Pokorny’s article for an [in-depth explanation on the negative impacts of using an index as a key](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318). If you choose not to assign an explicit key to list items then React will default to using indexes as keys.
    ```
    - don't indexes for keys if the order of items may change. This can negatively impact performance and may cause issues with component state. 

- Keys only make sense in the context of the surrounding array (what is surrounding array?).
    - A good rule of thumb is that elements inside the `map()` call need keys.

- Keys must only be unique among siblings; However, they don’t need to be globally unique.


## Forms
### Controlled components
- the React component that renders a form also controls what happens in that form on subsequent user input. An input form element whose value is controlled by React in this way is called a “controlled component”.
- It can sometimes be tedious to use controlled components, because you need to write an event handler for every way your data can change and pipe all of the input state through a React component. 
    - In these situations, you might want to check out [uncontrolled components](https://reactjs.org/docs/uncontrolled-components.html), an alternative technique for implementing input forms.

## Lifting state up
- In React, sharing state is accomplished by moving it up to the closest common ancestor of the components that need it. This is called “lifting state up”. 
    - replace `this.state.xxx` with `this.props.xxx` in the descendant component.
    - however, props are read-only, we can use `this.setState()` in the local/descendant components to update it
    - In React, this is usually solved by making a component “controlled”. 

- There should be a single “source of truth” for any data that changes in a React application. Usually, the state is first added to the component that needs it for rendering. Then, if other components also need it, you can lift it up to their closest common ancestor. Instead of trying to sync the state between different components, we should rely on the [top-down data flow](https://reactjs.org/docs/state-and-lifecycle.html#the-data-flows-down).

## Composition vs inheritance
- React has a powerful composition model, and we recommend using composition instead of inheritance to reuse code between components.

### Containment
- Some components don’t know their children ahead of time.  Such components can use the special `children` prop to pass children elements directly into their output

### Specialization
- Sometimes we think about components as being “special cases” of other components. In React, this is also achieved by composition, where a more “specific” component renders a more “generic” one and configures it with props



## Running questions
1. what is ReactDOM
2. when need to use `const` when creating a new object
3. when to use class component vs simple component
4. when to use `this.xx`
    - when set up/use a prop/state in a class (vs in a function) component
5. what is an `event`
6. what is a constructor?
7. what is mounting/unmounting? --> lifecycle methods?
8. [arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
9. What is W3C synthetic event?










## References
1. [React Official Docs](https://reactjs.org/docs/getting-started.html)
2. [React Tutorial: An Overview and Walkthrough](https://www.taniarascia.com/getting-started-with-react/)
3. [React tutorial from web.dev](https://web.dev/react/)