# Typescript

## Overview
* [Setup](#setup)
* [Types](#types)
* [Array](#array)
* [Interface](#interface)
* [Interface as function type](#interface-as-function-type)
* [Interface for array type](#interface-for-array-type)
* [Function](#function)
* [Any](#any)
* [Custom type](#custom-type)
* [Tuple](#tuple)
* [Enum](#enum)
* [Void](#void)
* [Never](#never)
* [Any](#any)
* [Unknown](#unknown)

## Setup

```bash
# installation
~ npm i -g typescript
~ npm i -g ts-node

# create tsconfig.json file
~ tsc --init

# execute
~ ts-node index.ts
```

## Types

```js
let str: string = "jimmy";

let num: number = 24;

let bool: boolean = false;

let u: undefined = undefined;

let n: null = null;

let obj: object = {x: 1};

let big: bigint = 100n;

let sym: symbol = Symbol("me");
```

## Array

```js
// way 1
let arr:string[] = ['1', '2'];

// way 2
let arr2:Array<string> = ['1', '2'];

// mixed types
let arr:(number | string)[] = [1, 'b', 2, 'c'];

```

Note:
  - 2 ways to define a type for an array.
  - Use `|` to define mixed types.


## Interface

```js
interface Person {
  name:string,
  age:number
}

// use as array
let arr:Person[] = [
  {
    name: 'Alice',
    age:55
  },
  {
    name: 'Tony',
    age: 77
  },
]

// use as single object
let obj:Person = {
  name: 'Amy',
  age: 45,
}
```

## Interface as function type

```js
interface KeyValueProcessor
{
    (key: number, value: string): void;
};

function addKeyValue(key:number, value:string):void {
  console.log('addKeyValue: key = ' + key + ', value = ' + value)
}

let kvp: KeyValueProcessor = addKeyValue;
kvp(1, 'Bill'); // addKeyValue: key = 1, value = Bill
```

## Interface for array type

```js
interface INumList {
  [index:number]: number
}

let numArr: INumList = [1, 2, 3];
console.log(numArr[0]); // 1
console.log(numArr[1]); // 2


interface IStringList {
  [index:string]: string
}

let strArr : IStringList = {};
strArr["TS"] = "Typescript";
strArr["JS"] = "JavaScript";

console.log(strArr["TS"]); // Typescript
console.log(strArr["JS"]); // JavaScript
```


## Function

```js
function sum(x: number, y: number): number {
  return x + y;
}

// optional param
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat'); // Tom Cat
let tom = buildName('Tom'); // Tom

// default value
function buildName(firstName: string, lastName: string = 'Cat') {
  return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat'); // Tom Cat
let tom = buildName('Tom'); // Tom Cat
```

## Any

```js
function push(array: any[], ...items: any[]) {
  items.forEach(function(item) {
      array.push(item);
  });
}

let arr: any[] = [];
push(arr, 3, 4, 5);
console.log(arr); // [3, 4, 5]

let arrMixed: string[] = ["a", "b"];
push(arrMixed, 44, false, null);
console.log(arrMixed); // ["a", "b", "c", 44, false, null]


function sum(a: any, b: any) {
  return a + b;
}

console.log(sum(10, 50)); // 60
console.log(sum('a', 'bc')); // abc
```

## Custom type

```js
type Combinable = string | number;

function add(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
   return a.toString() + b.toString();
  }
  return a + b;
}

console.log(add('a', 'b')); // ab
console.log(add('a', 123)); // a123
```

## Tuple
```js
let x: [string, number];

// x = ['hello', 'world']; // Error
// x = ['hello', 10, 10];  // Error
// x = [10, 'hello'];      // Error
// x = [123, 'hello'];     // Error
x = ['hello', 123];        // OK
console.log(x);


const employee: [number, string] = [1, "Steve"];

// access Tuple (method 1)
employee[0]; // 1
employee[1]; // Steve

// access Tuple (method 2)
const [id, username] = employee;

console.log(employee); // [1, "Steve"]
console.log(id); // 1
console.log(username); // Steve

// Optional Tuple
type Point = [number, number?, number?];

const x: Point = [10];
const xy: Point = [10, 20];
const xyz: Point = [10, 20, 10];

console.log(x.length); // 1
console.log(xy.length); // 2
console.log(xyz.length); // 3

console.log(x[0]); // 10
console.log(x[1]); // undefined


// Tuple object contains 1 number and n (0 or more) string
type RestTupleType = [number, ...string[]];
let restTuple: RestTupleType = [666, "Semlinker", "Kakuqo", "Lolo"];
console.log(restTuple[0]); // 666
console.log(restTuple[1]); // Semlinker


// Readonly Tuple
const point: readonly [number, number] = [10, 20];

console.log(point[0]); // OK. 10
point[0] = 1;          // Error. Cannot assign to '0' because it is a read-only property
```

Note:

- `Tuple` can contain two or more values of different data types.

## Enum

```js
// Numeric enum
enum PrintMedia {
  Newspaper,
  Newsletter,
  Magazine,
  Book
}

console.log(PrintMedia.Newspaper); // 0
console.log(PrintMedia.Newsletter); // 1
console.log(PrintMedia[0]); // Newspaper
console.log(PrintMedia[1]); // Newsletter



// String enum
enum PrintMedia {
  Newspaper = "NEWSPAPER",
  Newsletter = "NEWSLETTER",
  Magazine = "MAGAZINE",
  Book = "BOOK",
}

console.log(PrintMedia.Newspaper);     // NEWSPAPER
console.log(PrintMedia.Newsletter);    // NEWSLETTER
console.log(PrintMedia["Newsletter"]); // NEWSLETTER

// Heterogeneous enum
enum Status {
  Active = 'ACTIVE',
  Deactivate = 1,
  Pending
}

console.log(Status.Active); // ACTIVE
console.log(Status.Deactivate); // 1
console.log(Status.Pending); // 2

console.log(Status["Deactivate"]); // 1
console.log(Status[2]); // Pending
```

## Void

```js
// function without return
function fun(): void {
  console.log("this is TypeScript");
};
```

Note:
- Only `undefined` or `null` (if `strictNullChecks = false`) is assignable to variable with `void` type.


## Never

```js
function throwError(errorMsg: string): never {
  throw new Error(errorMsg);
}

function keepProcessing(): never {
  while (true) {
    console.log('I always does something and never ends.')
  }
}

// use case
type Foo = string | number;

function controlFlowAnalysisWithNever(foo: Foo) {
  if (typeof foo === "string") {
    // string logic goes here
  } else if (typeof foo === "number") {
    // number logic goes here
  } else {
    // purposely assign value to check type variable
    const check: never = foo;
  }
}
```

Note:

- The `never` type is used when you are sure that something is never going to occur.
- Difference between `never` and `void`:
  - The `void` type can have `undefined` or `null` as a value
  - The `never` cannot have any value.
- Use case:
  - We can purposely assign value to `never` type at `else` conditions, in case someone has updated the `Foo` type (ex: add new `boolean` type) but forgot to update the `controlFlowAnalysisWithNever` logic.
  - Typescript will detect the error.

## Any

```js
let a: any = 666;
a = "Semlinker";
a = false;
a = 66;
a = undefined;
a = null;
a = [];
a = {};

// default type is `any`, if there is no type specified
let something;
something = 'seven';
something = 7;
```

Note:

- A variable with `any` type can be reassigned to any different type.
- The `any` type will be treated as the default type, if no type is specified.


## Unknown

```js
let anything: any = 10;
let notSure: unknown = 10;

let d1: number = anything; // OK
// let d2: number = notSure; // Error

let resultAnything: any = notSure; // OK
let resultNotSure: unknown = anything; // OK
```

Note:
- Difference between `any` and `unknown`:
  - The `any` type:
    - Assign `any` value to `unknown` variable: OK.
    - Assign `any` value to `string` variable: OK.
    - Assign `any` value to `boolean` variable: OK.
  - The `unknown` type:
    - Assign `unknown` value to `any` variable: OK.
    - Assign `unknown` value to `string` variable: NG.
    - Assign `unknown` value to `boolean` variable: NG.


Refs:
- https://juejin.cn/post/7018805943710253086