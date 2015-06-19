# promises

Promises aren't a new and shiny idea. I use it every day in my AngularJS code. It's based on [kriskowal / q](https://github.com/kriskowal/q) library:

**A tool for creating and composing asynchronous promises in JavaScript.**

It's a library for asynchronous programming, to make our life easier. But, before I describe promises, I have to write something about callbacks.

## Callbacks and callback hell

Until I remember, JavaScript coders use callbacks for all browser-based asynchronous functions (setTimeout, XMLHttpRequest, etc.).

Look at naive example:

```javascript
console.log('start!');
setTimeout(function () {
  console.log('ping');
  setTimeout(function () {
    console.log('pong');
    setTimeout(function () {
      console.log('end!');
    }, 1000);
  }, 1000);
}, 1000);

// start!
// after 1 sec: ping
// .. 1 sec later: pong
// .. and: end!
```

We have simple code which prints some statements to the console. I used a **setTimeout** function here, to show callback functions passed to invoke later (1 sec here). It looks terrible and we have only 3 steps here. Let's imagine more steps. It will look like you build a pyramid, not nice, readable code. Awful, right? It's called **callback hell** and we have it everywhere.

[Callback Hell](http://callbackhell.com/)

## Promises

Support for promises is a very nice addition to the language. It's finally native in the ES6.

**Promises are a first class representation of a value that may be made available in the future.**

A promise can be:

* **fulfilled** - promise succeeded

* **rejected** - promise failed

* **pending** - not fulfilled or not rejected yet

* **settled** - fulfilled or rejected

Every returned **promise object** also has a **then** method to execute code when a promise is **settled**.

Yep, promise **object**, because..

**callbacks are functions, promises are objects.**

Callbacks are blocks of code to execute in response to.. something (event). Promises are objects which store an information about the state.

How does it look like? Let's see:

```javascript
new Promise((resolve, reject) => {
  // when success, resolve
  let value = 'success';
  resolve(value);

  // when an error occurred, reject
  reject(new Error('Something happened!'));
});
```

Promise calls its **resolve function** when it's fulfilled (success) and **reject function** otherwise (failure).

Promises are objects, so it's not passed as arguments like callbacks, it's **returned**. The return statement is an object which is a placeholder for the result, which will be available in the future.

Promises have just one responsibility-**they represent only one** event. Callbacks can handle multiple events, many times.

We can assign returned value (object) to the **let statement**:

```javascript
let promise = new Promise((resolve, reject) => {
  // when success, resolve
  let value = 'success';
  resolve(value);

  // when an error occurred, reject
  reject(new Error('Something happened!'));
});
```

As I mentioned above-promise object also has a **then** method to execute code when the promise is settled.

```javascript
promise.then(onResolve, onReject)
```

We can use this function to handle **onResolve** and **onReject** values returned by a promise. We can handle **success**, **failure** or **both**.

```javascript
let promise = new Promise((resolve, reject) => {
  // when success, resolve
  let value = 'success';
  resolve(value);

  // when an error occurred, reject
  reject(new Error('Something happened!'));
});

promise.then(response => {
  console.log(response);
}, error => {
  console.log(error);
});
// success
```

Our code above never executes **reject** function, so we can omit it for simplicity:

```javascript
let promise = new Promise(resolve => {
  let value = 'success';
  resolve(value);
});

promise.then(response => {
  console.log(response); // success
});
```

Handlers passed to **promise.then** don't just handle the result of the previous promise-they return is turned into a **new promise**.

```javascript
let promise = new Promise(resolve => {
  let value = 'success';
  resolve(value);
});

promise.then(response => {
  console.log(response); // success
  return 'another success';
}).then(response => {
  console.log(response); // another success
});
```

You can see, that the code based on promises is always *flat*. No more **callback hell**.

If you are only interested in rejections, you can omit the first parameter.

```javascript
let promise = new Promise((resolve, reject) => {
  let reason = 'failure';
  reject(reason);
});

promise.then(
  null,
  error => {
    console.log(error); // failure
  }
);
```

But is a more compact way of doing the same thing-**catch()** method.

```javascript
let promise = new Promise((resolve, reject) => {
  let reason = 'failure';
  reject(reason);
});

promise.catch(err => {
  console.log(err); // failure
});
```

If we have more than one **then()** call, the error is passed on until there is an error handler.

```javascript
let promise = new Promise(resolve => {
  resolve();
});

promise
  .then(response => {
    return 1;
  })
  .then(response => {
    throw new Error('failure');
  })
  .catch(error => {
    console.log(error.message); // failure
  });
```

We can even combine **one or more promises** into new promises without having to take care of ordering of the underlying asynchronous operations yourself.

```javascript
let doSmth = new Promise(resolve => {
    resolve('doSmth');
  }),
  doSmthElse = new Promise(resolve => {
    resolve('doSmthElse');
  }),
  oneMore = new Promise(resolve => {
    resolve('oneMore');
  });

Promise.all([
    doSmth,
    doSmthElse,
    oneMore
  ])
  .then(response => {
    let [one, two, three] = response;
    console.log(one, two, three); // doSmth doSmthElse oneMore
  });
```

**Promise.all()** takes an array of promises and when all of them are fulfilled, it put their values into the array.

There are two more functions which are useful:

* **Promise.resolve(value)** - it returns a promise which resolves to a **value** or returns **value** if **value** is already a promise
* **Promise.reject(value)** - returns rejected promise with **value** as **value**

## Pitfall

Promises have its pitfall as well. Let's image that when any exception is thrown within a **then** or the function passed to **new Promise**, will be silently disposed of **unless manually handled**.
