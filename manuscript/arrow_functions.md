# arrow functions

A new syntactic sugar which ES6 brings us soon, called **arrow functions** (also known as a fat arrow function). It's a shorter syntax compared to function expressions and lexically binds **this** value.

REMEMBER - **Arrow functions are always anonymous.**

## Syntactic sugar

How does it look? It's a signature:

```javascript
([param] [, param]) => {
 statements
}

           param => expression
(param1, param2) => { block }
```

..and it could be translated to:


```javascript
    () => { … }       // no argument
     x => { … }       // one argument
(x, y) => { … }       // several arguments

x => { return x * x } // block
x => x * x            // expression, same as above
```

Lambda expressions in JavaScript! Cool!

Instead of writing:


```javascript
[3, 4, 5].map(function (n) {
  return n * n;
});
```

..you can write something like this:


```javascript
[3, 4, 5].map(n => n * n);
```

Awesome. Isn't it? There is more!

## Fixed "this" = lexical "this"

**The value of this inside of the function is determined by where the arrow function is defined not where it is used.**

No more **bind**, **call** and **apply**! No more:


```javascript
var self = this;
```

It solves a major pain point (from my point of view) and has the added bonus of improving performance through JavaScript engine optimizations.

```javascript
// ES5
function FancyObject() {
 var self = this;

 self.name = 'FancyObject';
 setTimeout(function () {
  self.name = 'Hello World!';
 }, 1000);
}

// ES6
function FancyObject() {
  this.name = 'FancyObject';
  setTimeout(() => {
    this.name = 'Hello World!'; // properly refers to FancyObject
  }, 1000);
}
```

## The same function

* **typeof** returns **function**

* **instanceof** returns **Function**

## Limitations

* It's cannot be used as a constructor and will throw an error when used with **new**.

* Fixed **this** means that you cannot change the **value of this** inside of the function. It remains the same value throughout the entire lifecycle of the function.

* Regular functions can be **named**.

* Functions declarations are **hoisted** (can be used before they are declared).
