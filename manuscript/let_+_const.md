# let + const

First topic about ECMAScript 2015 is **let + const**. If you are familiar with JavaScript, you have probably known the term: **scope**. If you are not that lucky, don't worry about it. I'll explain that in a few words below.

Why I mentioned something about JavaScript **scope**? This is because **let** and **const** have a very strong connection with that word. Firstly, imagine and old way (and still valid) to declare a new variable in your JS code using ES5:

```javascript
// ES5
 
var a = 1;
 
if (1 === a) {
 var b = 2; 
}
 
for (var c = 0; c < 3; c++) {
 // …
}
 
function letsDeclareAnotherOne() {
 var d = 4;
}
 
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(d); // ReferenceError: d is not defined
 
// window
console.log(window.a); // 1
console.log(window.b); // 2
console.log(window.c); // 3
console.log(window.d); // undefined
```

1. We can see that variable **a** is declared as global. Nothing surprising.

2. Variable **b** is inside an **if block**, but in JavaScript it doesn't create a new scope. If you are familiar with other languages, you can be disappointed, but this is JavaScript and it works as you see.

3. The next statement is a **for loop**. **C** variable is declared in this for loop, but also in the global scope.

4. Until variable **d** is declared in his own scope. It's inside a function and only **function creates new scopes**.

**Variables in JavaScript are hoisted to the top!**

**Hoisting** is JavaScript's default behavior of moving all declarations to the top of the current scope (**to the top of the current script** or **the current function**).

In JavaScript, a variable can be declared after it has been used. In other words - a variable can be used before it has been declared!

One more rule, more aware: JavaScript only hoists declarations, not initialization.

```javascript
// scope and variable hoisting
 
var n = 1;
 
(function () {
 console.log(n);
 var n = 2;
 console.log(n);
})();
 
console.log(n);
```

Let's look for the new keywords in JavaScript ECMAScript 2015: **let** and **const**.

## let

We can imagine that **let** is a new var statement. What is the difference? let is block scoped. Let's see an example:

```javascript
// ES6 — let
 
let a = 1;
 
if (1 === a) {
 let b = 2; 
}
 
for (let c = 0; c < 3; c++) {
 // …
}
 
function letsDeclareAnotherOne() {
 let d = 4;
}
 
console.log(a); // 1
console.log(b); // ReferenceError: b is not defined
console.log(c); // ReferenceError: c is not defined
console.log(d); // ReferenceError: d is not defined
 
// window
console.log(window.a); // 1
console.log(window.b); // undefined
console.log(window.c); // undefined
console.log(window.d); // undefined
```

As we can see, this time only variable **a** is declared as a global. **let** gives us a way to declare block scoped variables, which is undefined outside it.

**I use Chrome (stable version) with #enable-javascript-harmony flag enabled. Visit chrome://flags/#enable-javascript-harmony, enable this flag, restart Chrome and you will get many new features.**

You can also use **BabelJS repl** or **Traceur repl** and compare results.

## const

**const** is single-assignment and like a **let**, block-scoped declaration.

```javascript
// ES6 const
 
{
 const PI = 3.141593;
 PI = 3.14; // throws “PI” is read-only
}
 
console.log(PI); // throws ReferenceError: PI is not defined
```

const cannot be reinitialized. It will throw an Error when we try to assign another value.

Let's look for the equivalent in ES5:

```javascript
// ES5 const
 
var PI = (function () {
 var PI = 3.141593;
 return function () { return PI; };
})();
```

