# DevTools Extensions

[Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)

# Install & Create React App

> [Official Create React App Tutorial](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app)  
> [Official Create React App readme](https://github.com/facebook/create-react-app#create-react-app--)  
> [Official Create React App User Guide](https://create-react-app.dev/)

Install & create a new React app and install the necessary packages

```bash
# also installs dev server, webpack, babel, etc
# npx is a package runner tool
npx create-react-app app-name
```

> note: can't install globally anymore: https://stackoverflow.com/a/61841380
> ```bash
> # won't work:
> npm i -g create-react-app
> 
> # To uninstall it:
> npm uninstall -g create-react-app
>
> Some osx/Linux users may need to also remove the old npm using:
> rm -rf /usr/local/bin/create-react-app
> ```

```bash
cd app-name

# launch app with dev server
npm start
```

## For Production

```bash
# to run config for production deployment
npm run eject

# create an optimized build of your app in the build folder
npm run build 
```

# Naming Conventions

- use `on[Event]` names for Component props which represent events
- use `handle[Event]` for methods which handle the events (HTML attribute names should stay the same.)
-

# Components

- pieces of UI that tell React what to display on the screen
- **When data changes, React will update and re-render our components**
- `React.Component` is a subclass of React
- takes in parameters, called `props`, and returns a hierarchy of views to display via the `render` method.

`ShoppingList` is a React component class/React component type:

```js
class ShoppingList extends React.Component {
    /**
     * Returns a description (React element, JSX) of what you want to see on the screen.
     *
     * @returns {JSX.Element}
     */
    render() {
        return (
            // The <div /> syntax is transformed at build time to React.createElement('div')
            <div className="shopping-list">
                <h1>Shopping List for {this.props.name}</h1>
                <ul>
                    <li>Instagram</li>
                    <li>WhatsApp</li>
                    <li>Oculus</li>
                </ul>
            </div>
        );
    }
}

// Example usage: <ShoppingList name="Mark" />
```

The example above is equivalent to:

```js
return React.createElement('div', {className: 'shopping-list'},
    React.createElement('h1', /* ... h1 children ... */),
    React.createElement('ul', /* ... ul children ... */)
);
```

> https://reactjs.org/docs/react-api.html#createelement

## Function Components

Simpler way to write components that only contain a render method and don't have their own state.

Takes `props` as input and returns what should be rendered.

### Example

As Component class:

```js
/**
 * renders a single tic-tac-toe square as a <button>
 */
class Square extends React.Component {
    render() {
        return (
            <button
                className="square"
                onClick={() => {
                    /* 
                    This event handler calls this.props.onClick()
                    Since the Board passed onClick={() => this.handleClick(i)} to Square,
                         the Square calls the Board's handleClick(i) when clicked
                    In React terms, the Square components are now controlled components;
                         the Board has full control over them
                    */
                    this.props.onClick({value: 'X'})
                }}
            >
                {this.props.value}
            </button>
        );
    }
}
```

As Function component:

```js
/**
 * renders a single tic-tac-toe square as a <button>
 */
function Square(props) {
    return (
        <button className="square" onClick={() => {
            props.onClick()
        }}>
            {props.value}
        </button>
    );
}
```

# JSX

A syntax extension to JavaScript that produces React “elements” to render to the DOM

- describe what the UI should look like
- comes with the full power of JavaScript
- put any JavaScript expressions within braces inside JSX
- each React element is a JavaScript object that you can store in a variable or pass around in your program.
- `camelCase` property naming; `class` becomes `className` in JSX

We can now refer to the whole shopping list by writing `<ShoppingList />`. Each React component is encapsulated and can
operate independently; this allows you to build complex UIs from simple components

If a tag is empty, you may close it immediately with `/>`, like XML:

```jsx
const element = <img src={user.avatarUrl}/>;
```

## XSS

It is safe to embed user input in JSX:

```jsx
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

> By default, React DOM escapes any values embedded in JSX before rendering them. Everything is converted to a string
> before being rendered. This helps prevent XSS (cross-site-scripting) attacks.

## Fragments

> https://reactjs.org/docs/fragments.html

Avoid unnecessary wrapper HTML with React Fragments

```jsx
import React from 'react';

class Table extends React.Component {
    render() {
        return (
            <table>
                <tr>
                    <Columns/>
                </tr>
            </table>
        );
    }
}
```

### Bad

```jsx
// WRONG
class Columns extends React.Component {
    render() {
        return (
            // useless div element
            <div>
                <td>Hello</td>
                <td>World</td>
            </div>
        );
    }
}
```

### Good

Avoid unnecessary wrapper HTML

```jsx
// Good
class Columns extends React.Component {
    render() {
        return (
            <React.Fragment>
                <td>Hello</td>
                <td>World</td>
            </React.Fragment>
        );
    }
}
```

`<Fragment>` is also Good, just import `Fragment` first

```jsx
import React, {Fragment} from 'react';

class Columns extends React.Component {
    render() {
        return (
            <Fragment>
                <td>Hello</td>
                <td>World</td>
            </Fragment>
        );
    }
}
```

Empty tag to indicate a fragment works too

```jsx
class Columns extends React.Component {
    render() {
        return (
            <>
                <td>Hello</td>
                <td>World</td>
            </>
        );
    }
}
```

### Keyed Fragments

```jsx
function Glossary(props) {
    return (
        <dl>
            {props.items.map(item => (
                // Without the `key`, React will fire a key warning
                <React.Fragment key={item.id}>
                    <dt>{item.term}</dt>
                    <dd>{item.description}</dd>
                </React.Fragment>
            ))}
        </dl>
    );
}
```

---

# Portals

> https://reactjs.org/docs/portals.html

`ReactDOM.createPortal(child, container)`

- A way to render children into a DOM node that exists outside the DOM hierarchy of the parent component
    - For example, modals, pop-ups, and other overlays
- Behaves like a normal React child
    - The portal still exists in the React tree regardless of position in the DOM tree
- [Managing keyboard focus](https://reactjs.org/docs/accessibility.html#programmatically-managing-focus) becomes very
  important
- Don't forget accessibility: https://www.w3.org/TR/wai-aria-practices-1.1/#dialog_modal

```jsx
render()
{
    // React does *not* create a new div. It renders the children into `domNode`.
    // `domNode` is any valid DOM node, regardless of its location in the DOM.
    return ReactDOM.createPortal(
        this.props.children,
        domNode);
}
```

An event fired from inside a portal will propagate to ancestors in the containing React tree, even if those elements are
not ancestors in the DOM tree

For example:

```html

<html>
<body>
<div id="app-root"></div>
<div id="modal-root"></div>
</body>
</html>
```

A Parent component in `#app-root` would be able to catch an uncaught, bubbling event from the sibling node `#modal-root`

```jsx
// These two containers are siblings in the DOM
const appRoot = document.getElementById('app-root');
const modalRoot = document.getElementById('modal-root');

class Modal extends React.Component {
    constructor(props) {
        super(props);
        this.el = document.createElement('div');
    }

    componentDidMount() {
        /* The portal element is inserted in the DOM tree after
        the Modal's children are mounted, meaning that children
        will be mounted on a detached DOM node. If a child
        component requires to be attached to the DOM tree
        immediately when mounted, for example to measure a
        DOM node, or uses 'autoFocus' in a descendant, add
        state to Modal and only render the children when Modal
        is inserted in the DOM tree. */
        modalRoot.appendChild(this.el);
    }

    componentWillUnmount() {
        modalRoot.removeChild(this.el);
    }

    render() {
        return ReactDOM.createPortal(this.props.children, this.el);
    }
}

class Parent extends React.Component {
    constructor(props) {
        super(props);
        this.state = {clicks: 0};
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        // This will fire when the button in Child is clicked,    
        // updating Parent's state, even though button    
        // is not direct descendant in the DOM.    
        this.setState(state => ({clicks: state.clicks + 1}));
    }

    render() {
        return (
            <div onClick={this.handleClick}>
                <p>Number of clicks: {this.state.clicks}</p>
                <p>
                    Open up the browser DevTools
                    to observe that the button
                    is not a child of the div
                    with the onClick handler.
                </p>
                <Modal> <Child/> </Modal></div>
        );
    }
}

function Child() {
    // The click event on this button will bubble up to parent,  
    // because there is no 'onClick' attribute defined  
    return (
        <div className="modal">
            <button>Click</button>
        </div>
    );
}

const root = ReactDOM.createRoot(appRoot);
root.render(<Parent/>);
```

---

# State

Used to "remember" things.

React components can have state by setting `this.state `in their constructors.

`this.state` should be considered as private to a React component that it's defined in.

```js
class Square extends React.Component {
    constructor(props) {
        super(props);
        // initialize the state of the component
        this.state = {
            value: null,
        };
    }
}
```

It's often preferable to move state into the parent:

```js
/**
 * renders 9 tic-tac-toe squares
 */
class Board extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            // save state of the squares and init with null value
            squares: Array(9).fill(null),
        };
    }

    /**
     * render a square element
     *
     * @param i
     * @returns {JSX.Element}
     */
    renderSquare(i) {
        return <Square
            value={this.state.squares[i]}
            /* Since the Board passes onClick={() => this.handleClick(i)} to Square,
            the Square onClick calls the Board's handleClick(i) when clicked */
            onClick={() => this.handleClick(i)}
        />;
    }

    handleClick(i) {
        // create a copy of the squares array
        const squares = this.state.squares.slice();
        squares[i] = 'X';
        this.setState({squares: squares});
    }

    // etc..
}

/**
 * renders a single tic-tac-toe square as a <button>
 */
class Square extends React.Component {
    render() {
        return (
            <button
                className="square"
                onClick={() => {
                    /* 
                    This event handler calls this.props.onClick()
                    Since the Board passed onClick={() => this.handleClick(i)} to Square,
                         the Square calls the Board's handleClick(i) when clicked
                    In React terms, the Square components are now controlled components;
                         the Board has full control over them
                    */
                    this.props.onClick({value: 'X'})
                }}
            >
                {this.props.value}
            </button>
        );
    }
}
```

## Data Changes

In `handleClick`, we call `.slice()` to create a copy of the squares array to modify instead of modifying the existing
array.

### Data Change with Mutation (Mutable)

```js
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}
```

### Data Change without Mutation (Immutable)

```js
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanged, but newPlayer is {score: 2, name: 'Jeff'}

// Or if you are using object spread syntax proposal, you can write:
// var newPlayer = {...player, score: 2};
```

#### Benefits of Immutability

- Complex features become simple; e.g., retaining data for undo/redo, history, etc
- Easier to detect changes/compare
- Determining When to Re-Render in React; helps build "pure components" in React - easily know when component has
  changed and needs re-rendering

---

# Hooks

- Functions that let you “hook into” React state and lifecycle features from function components
- Provide a more direct API to props, state, context, refs, and lifecycle
- Don't work inside classes
- There are built-in hooks and you can write your own

## Hook Rules

> https://reactjs.org/docs/hooks-rules.html

- Only call Hooks at the **top level**
    - Don't call Hooks inside loops, conditions, or nested functions.
- Only call Hooks **from React function components**
    - Don't call Hooks from regular JavaScript functions.
    - You can call Hooks from your own custom Hooks

> Plugin to enforce these rules for you: `npm install eslint-plugin-react-hooks --save-dev`

## State Hook -- `useState`

> https://reactjs.org/docs/hooks-state.html

`useState` returns a pair: the **current state value** and **a function** that lets you update it. React will preserve
the state between re-renders.

```jsx
// Declare a new state variable "count"
// setCount is the function that will update count's state
// 0 is the initial value (state) of count
const [count, setCount] = useState(0);
```

- Unlike `this.state`, it replaces the old state with the new state instead of merging them
- Unlike `this.state`, the state here doesn't have to be an object
- The initial `useState` argument is only used during the first render
- Multiple `useState` calls must occur in the same order for every render

### Example

This example renders a counter. When you click the button, it increments the value.

```jsx
// must import it
import React, {useState} from 'react';

function Example() {
    // Declare a new state variable called "count"
    // 0 is the initial state
    const [count, setCount] = useState(0);
    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

## Effect Hook -- `useEffect`

> https://reactjs.org/docs/hooks-effect.html

- Lets you perform side effects in function components:
    - data fetching
    - setting up a subscription
    - manually changing the DOM
    - etc
- By default, `useEffect` **runs after the first render and after every update**
    - [Customize when it's run](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)
- Essentially a combination of the lifecycle hooks `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`
- Unlike `componentDidMount` or `componentDidUpdate`, effects scheduled with `useEffect` don't block the browser from
  updating the screen
    - If they do need to happen synchronously (i.e., measuring the layout), there is a separate `useLayoutEffect` Hook
      with identical API
- Side effects in React components may or may not require cleanup

```jsx
import React, {useState, useEffect} from 'react';

function Example() {
    const [count, setCount] = useState(0);

    // Similar to componentDidMount and componentDidUpdate:
    useEffect(() => {
        // Update the document title using the browser API
        document.title = `You clicked ${count} times`;
    });
    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

### Effects Without Cleanup

- Run some additional code after React has updated the DOM
    - Network requests
    - manual DOM mutations
    - logging
    - etc
- Don't need any clean up because we can run them and immediately forget about them

#### Example (Class)

Updates the document title right after React makes changes to the DOM:

```jsx
class Example extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }

    componentDidMount() {
        document.title = `You clicked ${this.state.count} times`;
    }

    componentDidUpdate() {
        document.title = `You clicked ${this.state.count} times`;
    }

    render() {
        return (
            <div>
                <p>You clicked {this.state.count} times</p>
                <button onClick={() => this.setState({count: this.state.count + 1})}>
                    Click me
                </button>
            </div>
        );
    }
}
```

##### Example (Hook)

Updates the document title right after React makes changes to the DOM:

```jsx
import React, {useState, useEffect} from 'react';

function Example() {
    const [count, setCount] = useState(0);

    // tell React your component needs to asynchronously do something after render
    useEffect(() => {
        document.title = `You clicked ${count} times`;
    });

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

### Effects With Cleanup

- Set up a subscription to some external data source
- It is important to clean up so that we don't introduce a memory leak
- Cleanup runs when:
    - the component unmounts
    - before running the effects next time
        - [how to opt out of this](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)

#### Example (Class)

A `ChatAPI` module that lets us subscribe to a friend's online status

#### Example (Hook)

A `ChatAPI` module that lets us subscribe to a friend's online status

```jsx
import React, {useState, useEffect} from 'react';

function FriendStatus(props) {
    const [isOnline, setIsOnline] = useState(null);

    // tell React your component needs to asynchronously do something after render
    useEffect(() => {
        function handleStatusChange(status) {
            setIsOnline(status.isOnline);
        }

        ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

        // return a function that specifies how to clean up after this effect
        // doesn't have to be a named function
        return function cleanup() {
            ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
        };
    });
    if (isOnline === null) {
        return 'Loading...';
    }
    return isOnline ? 'Online' : 'Offline';
}
```

## Ref Hook -- `useRef`

> https://reactjs.org/docs/hooks-reference.html#useref

Like a “box” that can hold a mutable value in its `.current` property.

- Returns a mutable `ref` object whose `.current` property is initialized to the passed argument (`initialValue`)
- `useRef` **doesn't notify you when its content changes**.
    - Mutating the `.current` property doesn't trigger a re-render
- The returned object will persist for the full lifetime of the component
- A common use case is to access a child imperatively
- Good for keeping any mutable value around similar to how you'd use instance fields in classes
- Creates a plain JavaScript object
    - The only difference between `useRef()` and creating a `{current: ...}` object is that `useRef` will give you the
      same ref object on every render

> Refs are primarily used as a way to access the DOM. If you pass a ref object to React with `<div ref={myRef} />`,
> React will set its `.current` property to the corresponding DOM node whenever that node changes

A common use case is to access a child imperatively:

```jsx
function TextInputWithFocusButton() {
    const inputEl = useRef(null);
    const onButtonClick = () => {
        // `current` points to the mounted text input element
        inputEl.current.focus();
    };
    return (
        <>
            <input ref={inputEl} type="text"/>
            <button onClick={onButtonClick}>Focus the input</button>
        </>
    );
```

# Lifecycle

[Lifecycle Diagram](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

## Common

![Screenshot_10](https://user-images.githubusercontent.com/105135920/169382408-c441e824-1251-41f1-8941-08841d5dfdc0.png)

## Uncommon

![Screenshot_9](https://user-images.githubusercontent.com/105135920/169382240-0985ca64-0175-4bed-82ac-4fc4390d37dc.png)

### Component phases

#### 1. Mounting

When an instance of a component is created and inserted into the DOM.

Special methods that are automatically called during these phases/events are **lifecycle hooks**

##### Mounting Methods

###### constructor

###### render

###### componentDidMount

#### 2. Update

Change the state of the component or give it new props

##### Update Methods

Called in order

###### render

###### componentDidMount

### 3. Unmount

When a component is removed from the DOM (deleted, etc)

##### Unmount Methods

###### componentWilUnmount

## Custom Hooks

> https://reactjs.org/docs/hooks-custom.html


---

# Pure Components (Optimizing Performance)

> [Optimizing Performance](https://reactjs.org/docs/optimizing-performance.html)

## `shouldComponentUpdate()`


