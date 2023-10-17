# Promise

মনে করুন আপনি একজন উঁচু স্তরের গায়ক, এবং আপনার ভক্তরা দিন রাত আপনার কাছে নতুন গানের জন্য আবদার করছে।

কিছুটা নিস্তার পাবার জন্য, আপনি তাদেরকে প্রতিস্রুতি দিলেন যে, যখনি গানটি প্রস্তুত হবে তখনি তাদের কাছে আপনি গানটি পাঠিয়ে দেবেন। আপনি আপনার ভক্তদের একটি লিস্ট দিলেন। তারা সেখানে তাদের ইমেইল এড্রেস পূরণ করে দিল, যাতে করে যখনি গানটা প্রস্তুত হবে, লিস্টে সাবস্ক্রাইব করে থাকে সকলেই যেন সাথে সাথে গানটি পেয়ে যায়। এবং যদি অনেক খারাপ কোন ঘটনা ঘটে, ধরুন, আপনার স্টুডিও তে আগুন লেগে গেল, যার কারণে আপনি গানটি প্রস্তুত করতে পারলেন না, লিস্টের সাবস্ক্রাইবাররা সেটার খবরও পেয়ে যাবে।

এতে করে সবাই সবাই খুশিঃ আপনি, কারণ আপনার ভক্তরা আপনাকে আর বিরক্ত করবেনা, এবং আপনার ভক্তরা, কারণ তারা গানটির প্রস্তুত হলেই পেয়ে যাবে।

প্রোগ্রামিং এর কিছু জিনিস এর বাস্তব উপমা (analogy) এটিঃ

1. একটি "উৎপাদনশীল কোড" (producing code) যা কোন কাজ করতে প্রয়োজনমত সময় নেয়। উদাহরণস্বরূপ, কিছু কোড যা নেটওয়ার্ক এর মাধ্যমে কোন ডাটা নিয়ে আসে। এটি হচ্ছে "গায়ক"।
2. একটি "গ্রাসকারী/ব্যবহারকারী কোড" (consuming code) যেটি "উৎপাদনশীল কোড" এর কাজ শেষ হওয়ার পর যে উত্তর তৈরী হচ্ছে সেটির জন্য অপেক্ষা করে বা চায়। অনেক ফাংশনেরই এই উত্তরের প্রয়োজন হতে পারে। এরা হচ্ছে "ভক্তরা"।
3. জাভাস্ক্রিপ্ট এর একটি বিশেষ অবজেক্ট *promise*, যা "উৎপাদনশীল কোড" এবং "গ্রাসকারী/ব্যবহারকারী কোড" এর সংযোগ ঘটায়। আমাদের উপমার ক্ষেত্রেঃ এটি হচ্ছে "সাবস্ক্রাইবার লিস্ট"। "উৎপাদনশীল কোড" তার কাজ সম্পন্ন করতে এবং উত্তর উৎপাদন করতে প্রয়োজন মত সময় নেয় এবং সেই উত্তরটি যখন প্রস্তুত হয়, তখন "promise" এই "উৎপাদনশীল কোড" এর সকল "গ্রাসকারী/ব্যবহারকারী কোড" এর কাছে উত্তরটিকে পাঠিয়ে দেয় বা সহজলভ্য করে তোলে।

আমাদের উপমাটি একদম পুরোপুরি সঠিক নয়, কারণ জাভাস্ক্রিপ্টের promise সমূহ একটি সাধারণ সাবস্ক্রাইবার লিস্ট থেকে অনেক জটিলঃ এদের আরো অনেক বৈশিষ্ট্য এবং অসুবিধাও আছে। কিন্তু আপাতত শুরু করার জন্য উপমাটি ব্যবহারযোগ্য।

একটি প্রমিস অবজেক্ট এর কন্সট্রাক্টর সিনট্যাক্স হলঃ

```js
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```

`new Promise` এর ভেতর যে ফাংশনটিকে আর্গুমেন্ট হিসেবে পাঠানো হচ্ছে, সেটিকে বলা হয় *এক্সিকিউটর* (executor)। যখন `new Promise` একটি প্রমিস অবজেক্ট তৈরি হয়, তখনই এক্সিকিউটর ফাংশনটি স্বয়ংক্রিয় ভাবে চলা শুরু করে। এই এক্সিকিউটরের মধ্যে উৎপাদনশীল কোডগুলো থাকে, যা কিছু সময় পর তার কাজ সম্পাদন করে ফলাফল উৎপাদন করে। উপর্যুক্ত উপমার ক্ষেত্রেঃ এক্সিকিউটর হচ্ছে "গায়ক"।

এই এক্সিকিউতরের দুইটি আর্গুমেন্ট, `resolve` এবং `reject` হচ্ছে কলব্যাক ফাংশন যা জাভাস্ক্রিপ্ট আমাদের স্বয়ংক্রিয়ভাবে দিয়ে দেয়। আমাদের লেখা কোড শুধু মাত্র এক্সিকিউটর ফাংশনের বডিতে থাকে।

যখন এক্সিকিউটর কোন ফলাফল পায়, ফলাফলটি তাড়াতাড়ি পেয়েছে নাকি দেরিতে পেয়েছে সেটা মুখ্য নয়, তখন এক্সিকিউটরটির উচিত নিচের যেকোন একটি কলব্যাক কে কল বা এক্সিকিউট করাঃ

- `resolve(value)` — যদি কাজটি সফলভাবে সমাপ্ত হয়, `value` হচ্ছে ফলাফল।
- `reject(error)` — যদি কোন ত্রুটি (error) ঘটে, `error` হচ্ছে error object।

সংক্ষেপেঃ এক্সিকিউটরটি স্বয়ংক্রিয় ভাবে চলে এবং কোন কাজ সম্পাদন করার চেষ্টা করে। চেষ্টা শেষ হলে, এটি `resolve` কল করে যদি কাজটি সফলভাবে সম্পাপ্ত হয় অথবা `reject` কল করে যদি কোন ত্রুটি দেখা দেয়।

`new Promise` কন্সট্রাক্টরটি যেই `promise` অবজেক্টটি রিটার্ন করে, তার অভ্যন্তরীণ বৈশিষ্ট্যসমূহ নিম্নরূপঃ

- `state` — প্রাথমিক পর্যায়ে `"pending"` থাকে, তারপর পরিবর্তিত হয়ে `"fulfilled"` হয় যখন `resolve` কল করা হয় অথবা `"rejected"` হয় যখন `reject` কল করা হয়।
- `result` — প্রাথমিক পর্যায়ে `undefined` থাকে, তারপর পরিবর্তিত হয়ে `value` হয় যখন `resolve(value)` কল করা হয় অথবা `error` হয় যখন `reject(error)` কল করা হয়।

অর্থাৎ এক্সিকিউটরটি শেষ পর্যন্ত `promise` টিকে নিচের যেকোন একটা স্টেট এ নিয়ে যায়ঃ

![](promise-resolve-reject.svg)

পরবর্তিতে আমরা দেখব কিভাবে "ফ্যানরা" এই পরিবর্তন গুলতে সাবস্ক্রাইব করতে পারে।

নিচে প্রমিস কন্সট্রাক্টর এবং একটি এক্সিকিউটর ফাংশন এর উদাহরণ দেয়া হল যেখানে এক্সিকিউটর ফাংশনের "উৎপাদনশীল কোড" এর কাজ শেষ করতে (`setTimeout` ব্যবহার করে) কিছু সময়ের প্রয়োজন হয়ঃ

```js run
let promise = new Promise(function(resolve, reject) {
  // the function is executed automatically when the promise is constructed

  // after 1 second signal that the job is done with the result "done"
  setTimeout(() => *!*resolve("done")*/!*, 1000);
});
```

উপর্যুক্ত কোডটি রান করে আমরা দুটি জিনিস দেখতে পাইঃ

1. এক্সিকিউটর ফাংশনটি স্বয়ংক্রিয়ভাবে এবং সাথে সাথে কল হচ্ছে (`new Promise` এর মাধ্যমে)
2. এক্সিকিউটর ফাংশনটি দুটি আর্গুমেন্ট গ্রহণ করছেঃ `resolve` এবং `reject`। এই ফাংশন দুটি আগে থেকেই জাভাস্ক্রিপ্ট ইঞ্জিন তৈরি করে রেখেছে, যার কারণে আমাদের এই দুটি ফাংশন নতুন করে তৈরি করতে হয় নি। এই ফাংশনগুলো আমরা যথাযথ সময়ে কল করব।

    এক সেকেন্ড "প্রসেসিং" এর পর, ফলাফল উৎপাদনের জন্য এক্সিকিউটরটি `resolve("done")` কল করে। এটি `promise` অবজেক্ট এর স্টেটকে পরিবর্তন করে দেয়ঃ

    ![](promise-resolve-1.svg)

এই উদাহরণটি একটি "fulfilled promise" এর উদাহরণ যেখানে উৎপাদনশীল কোডের কাজ সফলভাবে সম্পন্ন হয়েছে।

এখন আমরা একটি উদাহরণ দেখব যেখানে এক্সিকিউটরটি প্রমিসকে একটি ত্রুটি (error) সহ রিজেক্ট করে দেয়ঃ

```js
let promise = new Promise(function(resolve, reject) {
  // after 1 second signal that the job is finished with an error
  setTimeout(() => *!*reject(new Error("Whoops!"))*/!*, 1000);
});
```

`reject(...)` কলটি প্রমিস অবজেক্ট এর স্টেট কে `"rejected"` স্টেটে পাঠিয়ে দেয়ঃ

![](promise-reject-1.svg)

সংক্ষেপে, এক্সিকিউটরটি একটি কাজ সম্পাদন করবে (সাধারণত এমন কোনো কাজ যেটি শেষ করতে সময় লাগে) এবং তারপরে সংশ্লিষ্ট প্রমিস অবজেক্ট এর স্টেট পরিবর্তন করতে `resolve` অথবা `reject` কে কল করবে।

একটি প্রমিস যদি resolved অথবা rejected স্টেটে থাকে, তাহলে তাকে "setteled" বলে, যেখানে শুরুতে এটি ছিল একটি "pending" প্রমিস।

````smart header="There can be only a single result or an error"
The executor should call only one `resolve` or one `reject`. Any state change is final.

All further calls of `resolve` and `reject` are ignored:

```js
let promise = new Promise(function(resolve, reject) {
*!*
  resolve("done");
*/!*

  reject(new Error("…")); // ignored
  setTimeout(() => resolve("…")); // ignored
});
```

The idea is that a job done by the executor may have only one result or an error.

Also, `resolve`/`reject` expect only one argument (or none) and will ignore additional arguments.
````

```smart header="Reject with `Error` objects"
In case something goes wrong, the executor should call `reject`. That can be done with any type of argument (just like `resolve`). But it is recommended to use `Error` objects (or objects that inherit from `Error`). The reasoning for that will soon become apparent.
```

````smart header="Immediately calling `resolve`/`reject`"
In practice, an executor usually does something asynchronously and calls `resolve`/`reject` after some time, but it doesn't have to. We also can call `resolve` or `reject` immediately, like this:

```js
let promise = new Promise(function(resolve, reject) {
  // not taking our time to do the job
  resolve(123); // immediately give the result: 123
});
```

For instance, this might happen when we start to do a job but then see that everything has already been completed and cached.

That's fine. We immediately have a resolved promise.
````

```smart header="The `state` and `result` are internal"
The properties `state` and `result` of the Promise object are internal. We can't directly access them. We can use the methods `.then`/`.catch`/`.finally` for that. They are described below.
```

## Consumers: then, catch, finally

A Promise object serves as a link between the executor (the "producing code" or "singer") and the consuming functions (the "fans"), which will receive the result or error. Consuming functions can be registered (subscribed) using methods `.then`, `.catch` and `.finally`.

### then

The most important, fundamental one is `.then`.

The syntax is:

```js
promise.then(
  function(result) { *!*/* handle a successful result */*/!* },
  function(error) { *!*/* handle an error */*/!* }
);
```

The first argument of `.then` is a function that runs when the promise is resolved, and receives the result.

The second argument of `.then` is a function that runs when the promise is rejected, and receives the error.

For instance, here's a reaction to a successfully resolved promise:

```js run
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done!"), 1000);
});

// resolve runs the first function in .then
promise.then(
*!*
  result => alert(result), // shows "done!" after 1 second
*/!*
  error => alert(error) // doesn't run
);
```

The first function was executed.

And in the case of a rejection, the second one:

```js run
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// reject runs the second function in .then
promise.then(
  result => alert(result), // doesn't run
*!*
  error => alert(error) // shows "Error: Whoops!" after 1 second
*/!*
);
```

If we're interested only in successful completions, then we can provide only one function argument to `.then`:

```js run
let promise = new Promise(resolve => {
  setTimeout(() => resolve("done!"), 1000);
});

*!*
promise.then(alert); // shows "done!" after 1 second
*/!*
```

### catch

If we're interested only in errors, then we can use `null` as the first argument: `.then(null, errorHandlingFunction)`. Or we can use `.catch(errorHandlingFunction)`, which is exactly the same:


```js run
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

*!*
// .catch(f) is the same as promise.then(null, f)
promise.catch(alert); // shows "Error: Whoops!" after 1 second
*/!*
```

The call `.catch(f)` is a complete analog of `.then(null, f)`, it's just a shorthand.

### finally

Just like there's a `finally` clause in a regular `try {...} catch {...}`, there's `finally` in promises.

The call `.finally(f)` is similar to `.then(f, f)` in the sense that `f` always runs when the promise is settled: be it resolve or reject.

`finally` is a good handler for performing cleanup, e.g. stopping our loading indicators, as they are not needed anymore, no matter what the outcome is.

Like this:

```js
new Promise((resolve, reject) => {
  /* do something that takes time, and then call resolve/reject */
})
*!*
  // runs when the promise is settled, doesn't matter successfully or not
  .finally(() => stop loading indicator)
  // so the loading indicator is always stopped before we process the result/error
*/!*
  .then(result => show result, err => show error)
```

That said, `finally(f)` isn't exactly an alias of `then(f,f)` though. There are few subtle differences:

1. A `finally` handler has no arguments. In `finally` we don't know whether the promise is successful or not. That's all right, as our task is usually to perform "general" finalizing procedures.
2. A `finally` handler passes through results and errors to the next handler.

    For instance, here the result is passed through `finally` to `then`:
    ```js run
    new Promise((resolve, reject) => {
      setTimeout(() => resolve("result"), 2000)
    })
      .finally(() => alert("Promise ready"))
      .then(result => alert(result)); // <-- .then handles the result
    ```

    And here there's an error in the promise, passed through `finally` to `catch`:

    ```js run
    new Promise((resolve, reject) => {
      throw new Error("error");
    })
      .finally(() => alert("Promise ready"))
      .catch(err => alert(err));  // <-- .catch handles the error object
    ```

That's very convenient, because `finally` is not meant to process a promise result. So it passes it through.

We'll talk more about promise chaining and result-passing between handlers in the next chapter.


````smart header="We can attach handlers to settled promises"
If a promise is pending, `.then/catch/finally` handlers wait for it. Otherwise, if a promise has already settled, they just run:

```js run
// the promise becomes resolved immediately upon creation
let promise = new Promise(resolve => resolve("done!"));

promise.then(alert); // done! (shows up right now)
```

Note that this makes promises more powerful than the real life "subscription list" scenario. If the singer has already released their song and then a person signs up on the subscription list, they probably won't receive that song. Subscriptions in real life must be done prior to the event.

Promises are more flexible. We can add handlers any time: if the result is already there, they just execute.
````

Next, let's see more practical examples of how promises can help us write asynchronous code.

## Example: loadScript [#loadscript]

We've got the `loadScript` function for loading a script from the previous chapter.

Here's the callback-based variant, just to remind us of it:

```js
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}
```

Let's rewrite it using Promises.

The new function `loadScript` will not require a callback. Instead, it will create and return a Promise object that resolves when the loading is complete. The outer code can add handlers (subscribing functions) to it using `.then`:

```js run
function loadScript(src) {
  return new Promise(function(resolve, reject) {
    let script = document.createElement('script');
    script.src = src;

    script.onload = () => resolve(script);
    script.onerror = () => reject(new Error(`Script load error for ${src}`));

    document.head.append(script);
  });
}
```

Usage:

```js run
let promise = loadScript("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js");

promise.then(
  script => alert(`${script.src} is loaded!`),
  error => alert(`Error: ${error.message}`)
);

promise.then(script => alert('Another handler...'));
```

We can immediately see a few benefits over the callback-based pattern:


| Promises | Callbacks |
|----------|-----------|
| Promises allow us to do things in the natural order. First, we run `loadScript(script)`, and `.then` we write what to do with the result. | We must have a `callback` function at our disposal when calling `loadScript(script, callback)`. In other words, we must know what to do with the result *before* `loadScript` is called. |
| We can call `.then` on a Promise as many times as we want. Each time, we're adding a new "fan", a new subscribing function, to the "subscription list". More about this in the next chapter: [](info:promise-chaining). | There can be only one callback. |

So promises give us better code flow and flexibility. But there's more. We'll see that in the next chapters.
