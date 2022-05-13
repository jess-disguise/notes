# Arrow Functions
```js
function something (){
    return 'hello';
}

// same as 


```

# Inheritance
Must call parent constructor with `super(props)`
```js
class Square extends React.Component {
    constructor(props) {
        super(props);
    }
}
```

# Arrays 
## Shallow Copies (`slice`, `concat`, `create`, etc) 
Is a _[shallow copy](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy)_ - copy basically by reference.

In JavaScript, **all standard built-in object-copy operations create shallow copies** rather than deep copies (`spread` syntax, `Array.prototype.concat()`, `Array.prototype.slice()`, `Array.from()`, `Object.assign()`, and `Object.create()`).

> A shallow copy of an object is a copy whose properties share the same references (point to the same underlying values) as those of the source object from which the copy was made.
>
> When you change either the source or the copy, you may also cause the other object to change too.
>
> Selectively changing the value of a shared property of an existing element in an object is different from assigning a completely new value to an existing element.

### Example
Consider the following example, in which an `ingredients_list` array object is created, and then an `ingredients_list_copy` object is created by copying that `ingredients_list` object.
```js
let ingredients_list = ["noodles",{"list":["eggs","flour","water"]}];

let ingredients_list_copy = Array.from(ingredients_list);
console.log(JSON.stringify(ingredients_list_copy));
// ["noodles",{"list":["eggs","flour","water"]}]
```

Changing the value of the list property in `ingredients_list_copy` will also cause the list property to change in the `ingredients_list` source object.

```js
ingredients_list_copy[1].list = ["rice flour","water"]
console.log(ingredients_list[1].list);
// Array [ "rice flour", "water" ]
console.log(JSON.stringify(ingredients_list));
// ["noodles",{"list":["rice flour","water"]}]
```

Assigning a completely new value to the first element in `ingredients_list_copy` will not cause any change to the first element in the `ingredients_list` source object.
```js
ingredients_list_copy[0] = "rice noodles"
console.log(ingredients_list[0])
// noodles
console.log(JSON.stringify(ingredients_list_copy));
// ["rice noodles",{"list":["rice flour","water"]}]
console.log(JSON.stringify(ingredients_list));
// ["noodles",{"list":["rice flour","water"]}]
```

# `this`
> https://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/

## Core
```js
function hello(thing) {
  console.log(this + " says hello " + thing);
}

hello.call("Yehuda", "world") //=> Yehuda says hello world
```

## Simple Function
```js
function hello(thing) {
  console.log("Hello " + thing);
}

// this:
hello("world")

// desugars to:
hello.call(window, "world");

// ## In strict mode: ##
// this:
hello("world")

// desugars to:
hello.call(undefined, "world");
```

**A function invocation like `fn(...args)` is the same as `fn.call(window [ES5-strict: undefined], ...args)`**

This is also true about functions declared inline:  
`(function() {})()` is the same as `(function() {}).call(window [ES5-strict: undefined)`

## Member Functions
```js
var person = {
  name: "Brendan Eich",
  hello: function(thing) {
    console.log(this + " says hello " + thing);
  }
}

// this:
person.hello("world")

// desugars to this:
person.hello.call(person, "world");
```

### Attached Dynamically
```js
function hello(thing) {
console.log(this + " says hello " + thing);
}

person = { name: "Brendan Eich" }
person.hello = hello;

person.hello("world") // still desugars to person.hello.call(person, "world")

hello("world") // "[object DOMWindow]world"
```

## Using Function.prototype.bind
A simple closure trick can convert a function into one with an unchanging `this`
```js
var person = {
  name: "Brendan Eich",
  hello: function(thing) {
    console.log(this.name + " says hello " + thing);
  }
}

var boundHello = function(thing) { return person.hello.call(person, thing); }

boundHello("world");
```
> Even though our `boundHello` call still desugars to `boundHello.call(window, "world")`, we turn right around and use our primitive `call` method to change the `this` value back to what we want it to be.

We can make this trick general-purpose with a few tweaks:
```js
var bind = function(func, thisValue) {
    return function() {
        return func.apply(thisValue, arguments);
    }
}

var boundHello = bind(person.hello, person);
boundHello("world") // "Brendan Eich says hello world"
```

`arguments` is an Array-like object that represents all of the arguments passed into a function. 

The `apply` method works exactly like the `call` primitive, except that it takes an Array-like object instead of listing the arguments out one at a time.

Our `bind` method simply returns a new function. When it is invoked, our new function simply invokes the original function that was passed in, setting the original value as `this`. It also passes through the arguments.

### bind()
Because this was a somewhat common idiom, ES5 introduced a new method `bind` on all `Function` objects that implements this behavior:
```js
var boundHello = person.hello.bind(person);
boundHello("world") // "Brendan Eich says hello world"
```

This is most useful when you need a raw function to pass as a callback:
```js
var person = {
name: "Alex Russell",
hello: function() { console.log(this.name + " says hello world"); }
}

$("#some-div").click(person.hello.bind(person));

// when the div is clicked, "Alex Russell says hello world" is printed
```