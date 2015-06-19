# default + rest + spread

ECMAScript 2015 functions made a significant progress, taking into account years of complaints. The result is a number of improvements that make programming in JavaScript **less error-prone** and **more powerful**.

Let's see three new features which give us **extended parameter handling**.

## default

It's a simple, little addition that makes it much easier to handle function parameters. Functions in JavaScript allow any number of parameters to be passed regardless of the number of declared parameters in the function definition. You probably know **commonly seen pattern** in current JavaScript code:

```javascript
function inc(number, increment) {
  // set default to 1 if increment not passed
  // (or passed as undefined)
  increment = increment || 1;
  return number + increment;
}

console.log(inc(2, 2)); // 4
console.log(inc(2));    // 3
```

The logical **OR operator** (||) always returns the second operand when the first is falsy.

**ES6** gives us a way to set default function parameters. Any parameters with a default value are considered to be optional.

ES6 version of **inc** function looks like this:

```javascript
function inc(number, increment = 1) {
  return number + increment;
}

console.log(inc(2, 2)); // 4
console.log(inc(2));    // 3
```

You can also set default values to parameters that appear **before arguments** without default values:

```javascript
function sum(a, b = 2, c) {
  return a + b + c;
}

console.log(sum(1, 5, 10));         // 16 -> b === 5
console.log(sum(1, undefined, 10)); // 13 -> b as default
```

You can even execute a function to set default parameter. It's not restricted to **primitive values**.

```javascript
function getDefaultIncrement() {
  return 1;
}

function inc(number, increment = getDefaultIncrement()) {
  return number + increment;
}

console.log(inc(2, 2)); // 4
console.log(inc(2));    // 3
```

## rest

Let's rewrite **sum** function to handle all arguments passed to it (without validation - just to be clear). If we want to use **ES5**, we probably also want to use **arguments** object.

```javascript
function sum() {
   var numbers = Array.prototype.slice.call(arguments),
       result = 0;
   numbers.forEach(function (number) {
       result += number;
   });
   return result;
}

console.log(sum(1));             // 1
console.log(sum(1, 2, 3, 4, 5)); // 15
```

But it's not obvious that the function is capable of handling any parameters. W have to scan body of the function and find **arguments** object.

**ECMAScript 6** introduces **rest** parameters to help us with this and other pitfalls.

**arguments - contains all parameters including named parameters**

Rest parameters are indicated by three dots **…** preceding a parameter. Named parameter becomes an **array** which contain the rest of the parameters.

**sum** function can be rewritten using ES6 syntax:

```javascript
function sum(…numbers) {
  var result = 0;
  numbers.forEach(function (number) {
    result += number;
  });
  return result;
}

console.log(sum(1)); // 1
console.log(sum(1, 2, 3, 4, 5)); // 15
```

Restriction: **no other named arguments can follow in the function declaration.**

```javascript
function sum(…numbers, last) { // causes a syntax error
  var result = 0;
  numbers.forEach(function (number) {
    result += number;
  });
  return result;
}
```

## spread

The **spread** is closely related to rest parameters, because of **…** (three dots) notation. It allows to split an array to single arguments which are passed to the function as separate arguments.

Let's define our **sum** function an pass **spread** to it:

```javascript
function sum(a, b, c) {
  return a + b + c;
}

var args = [1, 2, 3];
console.log(sum(…args)); // 6
```

ES5 equivalent is:

```javascript
function sum(a, b, c) {
  return a + b + c;
}

var args = [1, 2, 3];
console.log(sum.apply(undefined, args)); // 6
```

Instead using an **apply** function, we can just type **…args** and pass all array argument separately.

We can also mix standard arguments with spread operator:

```javascript
function sum(a, b, c) {
  return a + b + c;
}

var args = [1, 2];
console.log(sum(…args, 3)); // 6
```

The result is the same. First two arguments are from args array, and the last passed argument is 3.
