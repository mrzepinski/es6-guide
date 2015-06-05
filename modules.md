# modules

Today we have a couple of ways to create **modules**, export & import them. JavaScript doesn't have any built-in module loader yet. Upcoming ECMAScript 2015 standard gives us a reason to make people happy. Finally ;)

We have third party standards: **CommonJS** and **AMD**. The most popular, but unfortunately incompatible standards for module loaders.

**CommonJS is known from [Node.js](https://nodejs.org/). It's mostly dedicated for servers and it supports synchronous loading. It also has a compact syntax focused on export and require keywords.**

**AMD and the most popular implementation - [RequireJS](http://requirejs.org/) are dedicated for browsers. AMD supports asynchronous loading, but has more complicated syntax than CommonJS.**

The goal for ES6 is (was) to mix these two standards and make both user groups happy.

ES6 gives us an easy syntax and support for asynchronous and configurable module loading.

**Async model** — no code executes until requested modules are available and processed.

## Named export

Modules can export multiple objects, which could be simple variables or functions.

```javascript
export function multiply (x, y) {
  return x * y;
};
```
We can also export a function stored in a variable, but we have to wrap the variable in a set of curly braces.
```javascript
var multiply = function (x, y) {
  return x * y;
};
export { multiply };
```
We can even export many objects and like in the above example — we have to wrap exported statements in a set of curly braces if we use one export keyword.

```javascript
export hello = 'Hello World';
export function multiply (x, y) {
  return x * y;
};
// === OR ===
var hello = 'Hello World',
    multiply = function (x, y) {
      return x * y;
    };
export { hello, multiply };
```
Let's just imagine that we have **modules.js** file with all exported statements. To import them in another file (**in the same directory**) we use … **import { .. } from** .. syntax:

```javascript
import { hello } from 'modules';
```
**We can omit .js extension just like in CommonJS and AMD.**

We can even import many statements:

```javascript
import { hello, multiply } from 'modules';
```
Imports may also be **aliased**:

```javascript
import { multiply as pow2 } from 'modules';
```
..and use **wildcard** (*) to import all exported statemets:

```javascript
import * from 'modules';
```
## Default export

In our module we can have many named exports, but we can also have a **default export**. It's because our module could be a large library and with default export we can import then an entire module. It could be also useful when our module has single value or model (class / constructor).

**One default export per module.**

```javascript
export default function (x, y) {
  return x * y;
};
```
This time we don’t have to use curly braces for importing and we have a chance to name imported statement as we wish.

```javascript
import multiply from 'modules';
// === OR ===
import pow2 from 'modules';
// === OR ===
...
```

**Module can have both named exports and a default export.**
```javascript
// modules.js
export hello = 'Hello World';
export default function (x, y) {
  return x * y;
};
// app.js
import pow2, { hello } from 'modules';
```
**The default export is just a named export with the special name default.**

```javascript
// modules.js
export default function (x, y) {
  return x * y;
};
// app.js
import { default } from 'modules';
```

## API

In addition, there is also a programmatic API and it allows to:

* Programmatically work with modules and scripts

* Configure module loading

**[SystemJS](https://github.com/systemjs/systemjs) — universal dynamic module loader — loads ES6 modules, AMD, CommonJS and global scripts in the browser and NodeJS. Works with both Traceur and Babel.**

Module loader should support:
* Dynamic loading

* Global namespace isolation

* Nested virtualization

* Compilation hooks



