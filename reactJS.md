# DevTools Extensions
[Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)

# Install & Create React App
> [Official Create React App Tutorial](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app)  
> [Official Create React App readme](https://github.com/facebook/create-react-app#create-react-app--)  
> [Official Create React App User Guide](https://create-react-app.dev/)

Install+create a new react app and install the needed packages
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

# About React

# Naming Conventions
In React, it’s conventional to use `on[Event]` names for Component props which represent events and `handle[Event]` for the methods which handle the events. (HTML attribute names should stay the same.)


## Components
Components are pieces of UI that tell react what to display on the screen. **When our data changes, React will efficiently update and re-render our components**

`ShoppingList` is a React component class, or React component type. 
> A component takes in parameters, called `props`, and returns a hierarchy of views to display via the `render` method.
`React.Component` is a subclass
```js
class ShoppingList extends React.Component {
    /**
     * Returns a description (React element, JSX) of what you want to see on the screen.
     * 
     * @returns {JSX.Element}
     */
    render() {
        return (
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
The `<div />` syntax is transformed at build time to `React.createElement('div')`.   
The example above is equivalent to:
```js
return React.createElement('div', {className: 'shopping-list'},
    React.createElement('h1', /* ... h1 children ... */),
    React.createElement('ul', /* ... ul children ... */)
);
```
> https://reactjs.org/docs/react-api.html#createelement

## JSX
JSX comes with the full power of JavaScript. You can put any JavaScript expressions within braces inside JSX. 

Each React element is a JavaScript object that you can store in a variable or pass around in your program.

We can now refer to the whole shopping list by writing `<ShoppingList />`. Each React component is encapsulated and can operate independently; this allows you to build complex UIs from simple components

## Function Components
Simpler way to write components that only contain a render method and don’t have their own state.

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
                         the Square calls the Board’s handleClick(i) when clicked
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
        <button className="square" onClick={() => {props.onClick()}}>
            {props.value}
        </button>
    );
}
```

# State
Used to "remember" things.

React components can have state by setting `this.state `in their constructors. `this.state` should be considered as private to a React component that it’s defined in.
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
            the Square onClick calls the Board’s handleClick(i) when clicked */
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
                         the Square calls the Board’s handleClick(i) when clicked
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
In `handleClick`, we call `.slice()` to create a copy of the squares array to modify instead of modifying the existing array.

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
- Determining When to Re-Render in React; helps build "pure components" in React - easily know when component has changed and needs re-rendering


# Pure Components (Optimizing Performance)
> [Optimizing Performance](https://reactjs.org/docs/optimizing-performance.html)

## `shouldComponentUpdate()`
TODO: notes

