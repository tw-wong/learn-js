# learn-js
 Some useful information related with js. 

## Overview
* [Promises, async/await](#promises-asyncawait)

## Promises, async/await
```javascript
let promise = new Promise(function(resolve, reject) {
  // executor
});
```
* When the executor obtains the result, it will call one the callbacks (`resolve, reject`).
* `resolve(value)` - if job finished successfully, `value` is the result.
* `reject(error)` - if an error occurred, `error` is the error object.
* The `promise` object has `state` property (pending, fulfilled, rejected).
  * Promise `state = pending` - Initial status.
  * Promise `state = fulfilled` - Status change to "fulfilled" when `resolve` is called.
  * Promise `state = rejected` - Status change to "rejected" when `reject` is called.

```javascript
// Promise job calls "resolve" callback.

let promise = new Promise(function(resolve, reject) {
  // after 1 seconds, this job is done with the result "completed!".
  setTimeout(() => resolve("completed!"), 1000);
});

// resolve runs the first function in .then
promise.then(
  result => alert(result), // shows "completed!" after 1 second
  error => alert(error) // doesn't run
);
```
* Example of Promise job calls "resolve" callback.
* `.then` has 2 arguments (result, error).
  * First argument `result` function that runs when promise is resolved and receives the result.
  * Second argument `error` function that runs when promise is rejected and receives the error.

```javascript
// Promise job calls "reject" callback.

let promise = new Promise(function(resolve, reject) {
  // after 1 seconds, this job is done with an error.
  
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// resolve runs the second function in .then
promise.then(
  result => alert(result), // doesn't run
  error => alert(error) // shows "Error: Whoops!" after 1 second
);
```
* Example of Promise job calls "reject" callback.

## Promise then, catch, finally
```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("completed!"), 1000);
});

promise
  .then(result => alert(result)) //show first.
  .catch(error => alert(error))  //doesn't run.
  .finally(() => alert('show after .then')); //show after.
```
* `.finally` will call after `.then`.

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("completed!"), 1000);
});

promise
  .finally(() => alert('show before .then')) //show first.
  .then(result => alert(result)) //show after.
  .catch(error => alert(error));  //doesn't run.  
```

* `.finally` will call before `.then`.

## Spread Operator (...)