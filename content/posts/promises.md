---
title: "Promises in JavaScript"
date: 2019-05-27T20:43:04+01:00
draft: false
description: "What are Promises in JavaScript, their syntax and more."
tags: [javascript]
---

Promises might be tricky and I consider them as a high level concept in JavaScript.
I'm using this blog post to investigate more about them myself and teach what I've learned, if you know anything I din't metion and it's important, Hit me up on [Twitter](https://twitter.com/tiago_taquelim).

Promises let you create asynchronous methods that **return values like synchronous methods**: instead of immediately returning the final value, **the asynchronous method returns a promise to supply the value at some point in the future**.

> "JavaScript Object that represents the eventual completion (or failure) of an asynchronous operation, and its resulting value."

This is the basic syntax for a promise:

```js
let promise = new Promise(function(resolve, reject) {
	setTimeout(() => resolve("Done"), 3000);
});
```

> ```new Promise(executor);```

-   The ```executor``` is a callback function that receives `resolve` and `reject` as arguments.
-   The executor normally **initiates some asynchronous work**, and then, once that completes, **either calls the resolve** function to resolve the promise or **else rejects it if an error occurred**.

So after the "work" is done the promise runs `resolve(value)` or `reject(value)`

![Graph](https://javascript.info/article/promise-basics/promise-resolve-reject@2x.png)

For example on the code example shown before the state after 1 second would be `fulfilled`:

![Success](https://javascript.info/article/promise-basics/promise-resolve-1@2x.png)

On the other hand, this code will be rejected with an error:

```js
let promise = new Promise(function(resolve, reject) {
	setTimeout(() => reject(new Error("Error")), 1000);
});
```

![Error](https://javascript.info/article/promise-basics/promise-reject-1@2x.png)

In conclusion, a promise takes a `executor` which should do something that usually takes time and then call `resolve` or `reject` to change the value of the returning Promise Object.

Now **to consume the promise** we use the `.then()`, `.catch()` and `.finally()` methods.

### .then

This one is the most important to consume if the promise is fullfiled.

```js
promise.then(alert("Done"));
```

### .catch

This one is run if there is an error

```js
promise.catch(alert); // shows "Error: Whoops!" after 1 second
```

### finally

Just like there’s a finally clause in a regular `try {...} catch {...}`, there’s finally in promises.

`finally` is a good handler for performing **cleanup**, e.g. stopping our loading indicators, as they are not needed any more, no matter what the outcome is.

```js
new Promise((resolve, reject) => {
  /* do something that takes time, and then call resolve/reject */
})
  // runs when the promise is settled, doesn't matter successfully or not
  .finally(() => stop loading indicator)
  .then(result => show result, err => show error)

```

If you want to learn more about Promises and other JavaScript related topics, I can recommend [javascript.info](https://javascript.info), I tooks a lot example from there, and in my opinion is one of the best JavaScript docs.
