# learn-js
 Some useful information related with js. 

## Overview
* [Promise](#promise)
* [Promise then, catch, finally](#promise-then-catch-finally)
* [Promise chaining](#promise-chaining)
* [Returning a Promise](#returning-a-promise)
* [Promise.race](#promiserace)
* [Promise.all](#promiseall)
* [Async / Await](#async--await)
* [Spread operator](#spread-operator-)
* [Arrow function](#arrow-function)

## Promise
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

## Promise chaining
```javascript
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(10);
    }, 3 * 100);
});

p.then((result) => {
    console.log(result); //10
    return result * 2;
}).then((result) => {
    console.log(result); //20
    return result * 3;
});
```
* The return value from `resolve()` is passed to first `.then()` method and its return value will continue passed to second `.then` method (in sequence).

```javascript
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(10);
    }, 3 * 100);
});

p.then((result) => {
    console.log("first: " + result); // first: 10
    return result * 2;
})

p.then((result) => {
    console.log("second: " + result); // second: 10
    return result * 3;
})

p.then((result) => {
    console.log("third: " + result); // third: 10
    return result * 4;
});
```
* It has multiple handlers `.then()` for one promise (not in chaining).
* These handlers have no relationships thus they execute independently (first `.then` method result is not passed to second `.then` method).

## Returning a Promise
```javascript
let p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(10);
    }, 3 * 100);
});

p.then((result) => {
    console.log("print: " + result); // print: 10
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(result * 2); // 20
        }, 3 * 1000);
    });
}).then((result) => {
    console.log("print: " + result); // print: 20
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(result * 3); // 60
        }, 3 * 1000);
    });
}).then(result => console.log("print: " + result)); // print: 60
```
* This code pattern allows us to execute some tasks in sequence.

## Promise.race
```javascript
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The first promise has resolved');
        resolve(10);
    }, 3 * 1000);

});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The second promise has resolved');
        resolve(20);
    }, 2 * 1000);
});


Promise.race([p1, p2])
    .then(value => console.log(`Resolved: ${value}`))
    .catch(reason => console.log(`Rejected: ${reason}`));
```
* `Promise.race()` returns a promise that resolves to the value from the first settled promise (`p2`), regardless it is resolved or rejected.

## Promise.all

* `Promise.all()` returns a promise that resolves to an array of values from the input promises.

## Async / Await
```javascript
async function f() {
  return 1;
}

f().then(alert); // 1
```
* A function always returns a promise, when there is a word "async" before function.

```javascript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // wait until the promise resolves (*)

  alert(result); // "done!"
}

```
* Keyword `await` will wait until promise settles and returns its result.
* It works only inside `async` function.

```javascript
async function showServiceCost() {
    try {
      let user = await getUser(100);  
      let services = await getServices(user);
      let cost = await getServiceCost(services);
      console.log(`The service cost is ${cost}`); // The service cost is 300
    } catch(error) {
       console.log(error.message);
    }
}

async function getUser(userId) {
  console.log('Get user from the database.');
  
  return new Promise((resolve, reject) => {
  	setTimeout(() => resolve({
    	userId: userId, 
      username: 'John'
    }), 1000)
  });
}

async function getServices(user) {
  console.log(`Get services of ${user.username} from the API.`);
  
  return new Promise((resolve, reject) => {
  	setTimeout(() => resolve([
      'Email', 'VPN', 'CDN'
    ]), 1000)
  });
}

async function getServiceCost(services) {
  console.log(`Calculate service costs of ${services}.`);
  
  return new Promise((resolve, reject) => {
  	setTimeout(() => resolve(services.length * 100), 1000)
  });
}

showServiceCost();
```
* `async/await` keywords that build on top of Promise.
* If function returns a Promise, we can place the `awaits` keyword in front of the function call (ex: `let user = await getUser(100);`).

## Spread Operator (...)

## Arrow function