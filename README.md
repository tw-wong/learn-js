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
* [Object literal syntax extensions](#object-literal-syntax-extensions)
* [Computed property name](#computed-property-name)
* [Concise method syntax](#concise-method-syntax)
* [Default parameters](#default-parameters)
* [Rest parameters](#rest-parameters)
* [Spread operator](#spread-operator-)
* [Object spread](#object-spread)
* [Spread operator vs. Object.assign()](#spread-operator-vs-objectassign)
* [Enumerable properties](#enumerable-properties)
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
```javascript
// Promise.all with all resolved example
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The first promise has resolved');

        resolve(10);
    }, 1 * 1000);

});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The second promise has resolved');
        resolve(20);
    }, 2 * 1000);
});

Promise.all([p1, p2])
    .then(results => {
        const total = results.reduce((p, c) => p + c);

        console.log(`Results: ${results}`); // Results: 10,20
        console.log(`Total: ${total}`); // Total: 30
    });
```

* `Promise.all()` returns a promise that resolves to an array of values from the input promises.

```javascript
// Promise.all with reject example

const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The first promise has resolved');
        resolve(10);
    }, 1 * 1000);

});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The second promise has rejected');
        reject('Failed');
    }, 2 * 1000);
});

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log('The third promise has resolved');
        resolve(30);
    }, 3 * 1000);
});

Promise.all([p1, p2, p3])
    .then(console.log) // never execute
    .catch(console.log); // Failed
```

* Only `.catch()` method is executed because one of the Promise has rejected (`p2`).
* `.then()` method is not executed because one of the Promise has rejected (`p2`).

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

## Object literal syntax extensions
```javascript
function createMachine(name, status) {
    return {
        name,
        status
    };
}

let obj = createMachine('Jane', 'Active');
console.log(obj); // {name: "Jane", status: "Active"}
```

* JavaScript engine assigns the `name` and `status` property values of the `name` and `status` arguments.

## Computed property name
```javascript
let prefix = 'pre_';

let obj = {
    [prefix + 'name']: 'server',
    [prefix + 'hours']: 10000
};

console.log(obj); // {pre_hours: 10000, pre_name: "server"}
```
* Square brackets (`[]`) allow you to use the string literals and variables as the property names.

## Concise method syntax
```javascript
// Prior to ES6
let server = {
    name: 'Server',
    restart: function() {
        console.log('The' + this.name + ' is restarting...');
    }
};

// ES6
let server = {
    name: 'Server',
    restart() {
        console.log(`The ${this.name} is restarting...`);
    }
};
```
* Prior to ES6, we need to specify the name and full function definition (`restart: function() {}`).
* ES6 makes the syntax for making a method of the object literal more succinct by removing the colon (:) and the function keyword (`restart()`).

## Default parameters
```javascipt
function add(x=10, y=20) {
  return x + y;
}

console.log(add(undefined, undefined)); // 30
console.log(add()); // 30
console.log(add(undefined, 5)); // 15
console.log(add(25)); // 45
```
* pass `undefined` values if we want to use default value for first parameter (ex: `add(undefined, 5)`).

## Rest parameters
```javascript
function fn(a,b,...args) {
  console.log("a: "+ a);
  console.log("b: "+ b);
  console.log("args: "+ args);
}

// a: 1
// b: 2
// args: [3, 'A', 'B', 'C']
fn(1, 2, 3, 'A', 'B', 'C');

// a: 1
// b: 2
// args: []
fn(1, 2);

```
* Rest parameter means the parameter has a prefix of three dots (`...`).
* Rest parameter (`...args`) must be at the end of the argument list. 

## Spread Operator (...)
```javascript
const odd = [1,3,5];
const combined = [2,4,6, ...odd];
console.log(combined); // [2, 4, 6, 1, 3, 5]

const odd2 = [1,3,5];
const combined2 = [...odd2, 2,4,6];
console.log(combined2); // [1, 3, 5, 2, 4, 6]

const odd3 = [1,3,5];
const combined3 = [2, ...odd3, 4,6];
console.log(combined3); // [2, 1, 3, 5, 4, 6]
```
* Spread operator allows us to spread out elements of an iterable object.
* Spread operator can be anywhere, not necessary to be at last.

```javascript
let rivers = ['Nile', 'Ganges', 'Yangte'];
let moreRivers = ['Danube', 'Amazon'];

rivers.push(...moreRivers);
console.log(rivers); //["Nile", "Ganges", "Yangte", "Danube", "Amazon"]
```
* A better way to use array `push` method with spread operator to improve the readability of code.

```javascript
let numbers = [1, 2];
let moreNumbers = [3, 4];

let allNumbers = [...numbers, ...moreNumbers];
console.log(allNumbers); // [1, 2, 3, 4]
```
* Concatenate two or more arrays using spread operator.

## Object spread
```javascript
const circle = {
    radius: 10
};

const coloredCircle = {
    ...circle,
    color: 'black'
};

console.log(coloredCircle); //{ color: "black", radius: 10}
```
* ES2018 expands the spread operator for an object.

```javascript
const circle = {
    radius: 10
};

const clonedCircle = {...circle};

console.log(clonedCircle); // { radius: 10 }
```
* We can use the spread operator to clone the own enumerable properties of an object.

## Spread operator vs. Object.assign()
```javascript
class Circle {
    constructor(radius) {
        this.radius = radius;
    }
    set diameter(value) {
        this.radius = value / 2;
        console.log('SET ', value);
    }
    get diameter() {
        return this.radius * 2;
    }
}

let circle = new Circle(100);

let cloneCircle1 = Object.assign(circle, {
    diameter: 200
});

// Output:
// SET 200
```

* `Object.assign()` method triggers the setters method (`set diameter(value)`).

```javascript
const blueSquare = {
    length: 100,
};

// define a property with readonly (writable: false)
Object.defineProperty(blueSquare, 'color', {
    value: 'blue',
    enumerable: true,
    writable: false

});

// TypeError: Cannot assign to read only property 'color' of object '#<Object>'
const redSquare = Object.assign(blueSquare, {
    color: 'red'
});

############
const style = {
    color: 'green'
};

const greenSquare = {
    ...blueSquare,
    ...style
}; 

console.log(greenSquare); // {color: "green", length: 100}
```
* `Object.assign()` will throw error when assigning new value to `readonly` property.
* But spread operator (...) allow to assign a new value to `readonly` property.

## Enumerable properties
```javascript
const person = {
    firstName: 'John',
    lastName: 'Doe'
};

person.age = 25;

for (const key in person) {
    console.log(key);
}

// Output:
// firstName
// lastName
// age
```
* The `enumerable` attribute determines whether or not a property is accessible using the `for...in` loop or `Object.keys()` method.


```javascript
const person = {
    firstName: 'John',
    lastName: 'Doe'
};

person.age = 25;

Object.defineProperty(person, 'ssn', {
    enumerable: false,
    value: '123-456-7890'
});

for (const key in person) {
    console.log(key);
}

// Output:
// firstName
// lastName
// age

console.log(person.propertyIsEnumerable('firstName')); // true
console.log(person.propertyIsEnumerable('lastName')); // true
console.log(person.propertyIsEnumerable('age')); // true
console.log(person.propertyIsEnumerable('ssn')); // false
```
* The `ssn` property is created with the `enumerable` flag sets to `false`, therefore it does not show up in the `for...in` loop.
* ES6 provides a method `propertyIsEnumerable()` that determines whether or not a property is `enumerable`.

## Arrow function
```javascript'
// example function that adds 2 numbers (x and y).
let add = function(x,y) {
  return x + y;
}
console.log(add(10,20)); // 30

// arrow function example 01:
let add = (x,y) => x + y;
console.log(add(10,20)); // 30;

// arrow function example 02:
let add = (x, y) => { return x + y; };
console.log(add(10,20)); // 30;
```
* Example 01: arrow function has one expression `x + y` so it returns the result of the expression.
* Example 02: use the block syntax, so we need to specify the `return` keyword.

```javascript
// example function that sort the array.
let numbers = [4,2,6];
numbers.sort(function(a,b){ 
    return b - a; 
});
console.log(numbers); // [6,4,2]

// arrow function example 01:
let numbers = [4,2,6];
numbers.sort((a,b) => b - a);
console.log(numbers); // [6,4,2]

########

// example function that only have 1 parameter.
let names = ['John', 'Mac', 'Peter'];
let lengths = names.map(function(name){
  return name.length;
})

// arrow function example 02:
let lengths = names.map(name => name.length);
console.log(lengths);

// arrow function example 03:
let logDoc = () => console.log(window.document);
logDoc();
```
* Example 01: We must use parentheses `()` if there is more than 1 parameter `(a,b) =>`.
* Example 02: We can omit the parenthese if there is only 1 parameter `name =>`.
* Example 03: We must use parentheses if there is no parameter `() =>`.