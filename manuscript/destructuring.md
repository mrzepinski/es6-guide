# destructuring

**Destructuring** is one more little addition to the upcoming JavaScript standard, which helps us write code more flexibly and effectively.

It allows binding using pattern matching. We can use it for matching arrays and objects. It's similar to standard object look up and returns **undefined** when value is not found.

## arrays

Today it's common to see the code such as this.

```javascript
// ES5
var point = [1, 2];
var xVal = point[0],
    yVal = point[1];
 
console.log(xVal); // 1
console.log(yVal); // 2
```

ES6 gives us destructuring of arrays into individual variables during assignment which is intuitive and flexible.

```javascript
// ES6
let point = [1, 2];
let [xVal, yVal] = point;
 
console.log(xVal); // 1
console.log(yVal); // 2
// .. and reverse!
[xVal, yVal] = [yVal, xVal];
 
console.log(xVal); // 2
console.log(yVal); // 1
```

We can even omit some values..

```javascript
let threeD = [1, 2, 3];
let [a, , c] = threeD;
console.log(a); // 1
console.log(c); // 3
```

..and have nested array destructuring.

```javascript
let nested = [1, [2, 3], 4];
let [a, [b], d] = nested;
console.log(a); // 1
console.log(b); // 2
console.log(d); // 4
```

## objects

As well as the array syntax, ES6 also has the ability to destructure objects. It uses an object literal on the left side of an assignment operation. Object pattern is very similar to array pattern seen above. Let's see:

```javascript
let point = {
  x: 1,
  y: 2
};
let { x: a, y: b } = point;
console.log(a); // 1
console.log(b); // 2
```

It supports nested object as well as array pattern.

```javascript
let point = {
  x: 1,
  y: 2,
  z: {
    one: 3,
    two: 4
  }
};
let { x: a, y: b, z: { one: c, two: d } } = point;
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(d); // 4
```

## mixed

We can also mix objects and arrays together and use theirs literals.

```javascript
let mixed = {
  one: 1,
  two: 2,
  values: [3, 4, 5]
};
let { one: a, two: b, values: [c, , e] } = mixed;
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(e); // 5
```

But I think the most interesting is that we are able to use **functions** which return destructuring assignment.

```javascript
function mixed () {
  return {
    one: 1,
    two: 2,
    values: [3, 4, 5]
  };
}
let { one: a, two: b, values: [c, , e] } = mixed();
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(e); // 5
```

The same result! It gives us a lot of possibilities to use it in our code.

## attention!

**If the value of a destructuring assignment isn't match, it evaluates to undefined.**

```javascript
let point = {
  x: 1
};
let { x: a, y: b } = point;
console.log(a); // 1
console.log(b); // undefined
```

**If we try to omit var, let or const, it will throw an error, because block code can‘t be destructuring assignment.**

```javascript
let point = {
  x: 1
};
{ x: a } = point; // throws error
```

We have to wrap it in parentheses. Just that ☺

```javascript
let point = {
  x: 1
};
({ x: a } = point);
console.log(a); // 1
```



