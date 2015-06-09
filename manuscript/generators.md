# generators

**Generators** are simply subtypes of **Iterators** which I wrote about previously. They are a special kind of function that can be suspended and resumed, which is making a difference to iterators. Generators use **function*** and **yield** operators to work their magic.

**The yield operator returns a value from the function and when the generator is resumed, execution continues after the yield.**

**We also have to use function* (with star character) instead of a function to return a generator instance.**

**!!! Generators have been borrowed from Python language.**

**The most magical feature in ES6!**

Why? Take a look:

```javascript
function* generator () {
  yield 1; 
  // pause
  yield 2; 
  // pause
  yield 3; 
  // pause
  yield 'done?'; 
  // done
}
let gen = generator(); // [object Generator]
console.log(gen.next()); // Object {value: 1, done: false}
console.log(gen.next()); // Object {value: 2, done: false}
console.log(gen.next()); // Object {value: 3, done: false}
console.log(gen.next()); // Object {value: 'done?', done: false}
console.log(gen.next()); // Object {value: undefined, done: true}
console.log(gen.next()); // Object {value: undefined, done: true}
for (let val of generator()) {
  console.log(val); // 1
                    // 2
                    // 3
                    // 'done?'
}
```

As you can see, the generator has four **yield** statements. Each returns a value, pauses execution and moves to the next yield when **next()** method is called. Calling a function produces an object for controlling generator execution, a so-called **generator object**.

Use is similar to iterators. We have **next()** method as I mentioned above and we can even use it with **for-of** loop.

Below is an example of a generator called **random1_10**, which returns random numbers from 1 to 10.


```javascript
function* random1_10 () {
  while (true) {
    yield Math.floor(Math.random() * 10) + 1;
  }
}
let rand = random1_10();
console.log(rand.next());
console.log(rand.next());
// …
```

Generator has never ending **while** loop. It produces random numbers every time when you call **next()** method.

ES5 implementation:

```javascript
function random1_10 () {
  return {
    next: function() {
      return {
        value: Math.floor(Math.random() * 10) + 1,
        done: false
      };
    }
  };
}
let rand = random1_10();
console.log(rand.next());
console.log(rand.next());
// …
```

We can also mix generators together:

```javascript
function* random (max) {
  yield Math.floor(Math.random() * max) + 1;
}
function* random1_20 () {
  while (true) {
    yield* random(20);
  }
}
let rand = random1_20();
console.log(rand.next());
console.log(rand.next());
// …
```

**random1_20** generator returns random values from 1 to 20. It uses **random** generator inside to create random number each time when yield statement is reached.
