# strings

I gonna show you a couple of changes to **strings** in JavaScript, which will be available when ES6 comes. A syntactic sugar, which could be helpful in daily work.

## template strings

First, a **string interpolation**. Yep, template strings (finally) support string interpolation. ES6 brings us also support for **multi-line** syntax and **raw literals**.

```javascript
let x = 1;
let y = 2;
let sumTpl = `${x} + ${y} = ${x + y}`;

console.log(sumTpl); // 1 + 2 = 3
```

As you can see, we can inject values to string by using **${value}** syntax. Another thing to consider is **grave accent** - a char under the tilde (~) on a keyboard. A template literal string must be wrapped by it, to work properly.

The example above is an equivalent (in ES5) to simply (Babel version):

```javascript
var x = 1;
var y = 2;
var sumTpl = "" + x + " + " + y + " = " + (x + y);

console.log(sumTpl); // 1 + 2 = 3
```

This feature is very useful and almost removes the need for a template system.

Template strings provide also **multi-line syntax**, which is not legal in ES5 and earlier.

```javascript
let types = `Number
String
Array
Object`;
console.log(types); // Number
                    // String
                    // Array
                    // Object
```

ES5 equivalent:

```javascript
var types = "Number\nString\nArray\nObject";
console.log(types); // Number
                    // String
                    // Array
                    // Object
```

The last thing is access the **raw template string** content where backslashes are not interpreted. We don't have equivalent in ES5 here.

```javascript
let interpreted = 'raw\nstring';
let esaped = 'raw\\nstring';
let raw = String.raw`raw\nstring`;

console.log(interpreted);    // raw
                             // string
console.log(raw === esaped); // true
```

## extended support for Unicode

ES6 gives us full support for **Unicode** within strings and regular expressions. It's non-breaking addition allows to building global apps.

Let's see an example:

```javascript
let str = '𠮷';

console.log(str.length);             // 2
console.log(str === '\uD842\uDFB7'); // true
```

You can see that character **𠮷*** represented by two 16-bit code units. It's a surrogate pair in which we have a single code point represented by two code units. The length of that string is also 2.

**Surrogate pairs are used in UTF-16 to represent code points above U+FFFF.**

```javascript
console.log(str.charCodeAt(0)); // 55362
console.log(str.charCodeAt(1)); // 57271
```

The **charCodeAt()** method returns the 16-bit number for each code unit.

ES6 allows encoding of strings in UTF-16. JavaScript can now support work with surrogate pairs. It gives us also a new method **codePointAt()** that returns Unicode code point instead of Unicode code unit.

```javascript
console.log(str.codePointAt(0)); // 134071
console.log(str.codePointAt(1)); // 57271
console.log(str.codePointAt(0) === 0x20BB7); // true
```

It works the same as **charCodeAt()** except for non-BMP characters.

**BMP - Basic Multilingual Plane - the first 2^16 code points.**

**codePointAt()** returns full code point at the 0 **position**. codePointAt() and charCodeAt() return the same value for **position 1**.

We can also do a reverse operation with another new method added to ES6: **fromCodePoint()**.

```javascript
console.log(String.fromCodePoint(134071));  // "𠮷"
console.log(String.fromCodePoint(0x20BB7)); // "𠮷"
```

**Unicode code unit escape sequences consist of six characters, namely \u plus four hexadecimal digits, and contribute one code unit.**

**Unicode code point escape sequences consist of five to ten characters, namely
\u{ 1–6 hexadecimal digits }, and contribute one or two code units.**

Dealing with that two definitions, above example could be represented by one code point in **ES6**:

```javascript
// ES6
console.log('\u{20BB7}'); // 𠮷
console.log('\u{20BB7}' === '\uD842\uDFB7'); // true

// ES5
console.log('\u20BB7); // 7!
console.log('\u20BB7' === '\uD842\uDFB7'); // false
```

In ES5 we get an unexpected result when we try to match one single character using regular expression.

```javascript
console.log(/^.$/.test(str)); // false - length is 2
```

ES6 allows us to use new RegExp **u mode** to handle code points. It is simply a new **u** flag (u == Unicode).

```javascript
console.log(/^.$/u.test(str)); // true
```

Adding u flag allows to correctly match the string by characters instead of code units.

## strings are iterable

Strings are iterable by using the **for-of** loop which I will cover in more detail in iterators article later. I write about it now because it enumerate Unicode code points and each may comprise one or two characters.

```javascript
let str = 'abc\uD842\uDFB7';
console.log(str.length); // 5
for (let c of str) {
  console.log(c); // a
                  // b
                  // c
                  // 𠮷
}
```

We can also use **spread** operator to transform string into an array with full Unicode support.

```javascript
let str = 'abc\uD842\uDFB7';
let chars = […str];
console.log(chars); // ['a', 'b', 'c', '𠮷']
```

## new string methods

**repeat(n)** - string repeats by **n** times

```javascript
console.log('abc|'.repeat(3)); // 'abc|abc|abc|'
```

**startsWith(str, starts = 0) : boolean** - check if string starts with **str**, starting from **starts**

```javascript
console.log('ecmascript'.startsWith('ecma'));      // true
console.log('ecmascript'.startsWith('script', 4)); // true
```

**endsWith(str, ends = str.length) : boolean** - check if string ends with **str**, **ends** - where the string to be checked ends

```javascript
console.log('ecmascript'.endsWith('script'));  // true
console.log('ecmascript'.endsWith('ecma', 4)); // true
```

**includes(str, starts = 0) : boolean** - check if string contain **str**, starting from **starts**

```javascript
console.log('ecmascript'.includes('ecma'));      // true
console.log('ecmascript'.includes('script', 4)); // true
```
