# Typescript

## Overview
* [Setup](#setup)
* [Types](#types)
* [Array](#array)
* [Mixed types](#mixed-types)
* [Non-null assertion](#non-null-assertion)
* [Casting](#casting)
* [Class](#class)
* [Public, Private & Readonly](#public-private--readonly)
* [Protected](#protected)
* [Static](#static)
* [Abstract class](#abstract-class)
* [Interface](#interface)
* [Interface as function type](#interface-as-function-type)
* [Interface for array type](#interface-for-array-type)
* [Function](#function)
* [Any](#any)
* [Custom type](#custom-type)
* [Generics](#generics)
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
  - Use `|` to define mixed types (union).

## Mixed types

```js
// String
let test: string|number;
test = 'Hello'; // OK
test = 25; // OK
test = false; // NG

// Array
let arr: (string|number)[] = [];
arr.push('hello'); // OK
arr.push(12); // OK
arr.push(false); // NG

```

## Non-null assertion

```js
let word : string | null = null
const num = 1
if (num) {
    word = "Hello World"
}

// console.log(word.toLowerCase()) // Error: Object is possibly 'null'.ts(2531)

console.log(word!.toLowerCase())
```

Note:

- Use `!` to tell TypeScript we are certain word will never be null (or undefined).

## Casting

```js

// Use css selector and cast the variable as HTML form element.
const form = document.querySelector('.new-item-form') as HTMLFormElement;

console.log(form.children);
```

## Class

```js
class Invoice {
  client: string,
  info: string,
  amount: number,

  constructor(client: string, info: string, amount: number) {
    this.client = client;
    this.info = info;
    this.amount = amount;
  }
}

const invoiceOne = new Invoice('Ali', 'work on site A', 20);

```

## Public, Private & Readonly

```js
// method one
class Invoice {
  readonly client: string,
  private info: string,
  public amount: number,

  constructor(client: string, info: string, amount: number) {
    this.client = client;
    this.info = info;
    this.amount = amount;
  }

  format() {
    return `${this.client} owes USD${this.amount} for ${this.info}`
  }
}

// method two
class Invoice {
  constructor(
    readonly client: string,
    private info: string,
    public amount: number,
  ){}

  format() {
    return `${this.client} owes USD${this.amount} for ${this.info}`
  }
}

const invoiceOne = new Invoice('Ali', 'work on site A', 20);
```

Note:

- `readonly`: it can be accessed outside the class, but their value cannot be changed.

- `private`: it cannot be accessed from outside of its containing class.

- `public`: default modifiers. it can be freely accessed inside or outside the class.

## Protected

```js
class Greeter {
  public greet() {
    console.log("Hello, " + this.getName());
  }
  protected getName() {
    return "hi";
  }
}

class SpecialGreeter extends Greeter {
  public howdy() {
    // OK to access protected member here
    console.log("Howdy, " + this.getName());
  }
}
const g = new SpecialGreeter();
g.greet(); // Hello, hi
g.howdy(); // Howdy, hi

// g.getName(); // Property 'getName' is protected and only accessible within class 'Greeter' and its subclasses.ts(2445)
```

Note:

- `protected `members are only visible to subclasses of the class they’re declared in.

## Static

```js
class MyClass {
  private static x = 10;
  public static print = () => {
    return MyClass.x;
  }
}

// console.log(MyClass.x); // Property 'x' is private and only accessible within class 'MyClass'.ts(2341)
console.log(MyClass.print()) // 10
```

## Abstract class

```js
abstract class Base {
  abstract getName(): string;

  printName() {
    console.log("Hello, " + this.getName());
  }
}

class Derived extends Base {
  getName() {
    return "world";
  }
}

// cannot be directly instantiated
// const b = new Base();

const d = new Derived();
d.printName(); // Hello, world
```


## Interface

```js
// use as single object
let obj:Person = {
  name: 'Amy',
  age: 45,
}


// use as array
interface Person {
  name:string,
  age:number
}

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


// interface with method
interface IEmployee {
  empCode: number;
  empName: string;
  getSalary: (num: number) => number;
  getManagerName: (num: number) => string;
  getNickName: (name: string) => string;
}

let emp: IEmployee = {
  empCode: 123,
  empName: "Emp name",
  getSalary: (num) => {
    return num + 2;
  },
  getManagerName: (num) => {
    return num + 'abc';
  },
  getNickName: (str) => {
    return str;
  }
}

console.log(emp.getSalary(1)); // 3
console.log(emp.getManagerName(11)); // 11abc
console.log(emp.getNickName("Amy")); // Amy
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

## Generics

```js
// Example 1:
const addUID = <T>(obj: T) => {
  let uid = Math.floor(Math.random() * 100);
  return {...obj, uid};
}

let docOne = addUID({name: 'Hello', age: 30});
console.log(docOne); // { name: 'Hello', age: 30, uid: 54 }


// Example 2:
// only allow object with name property
const addUID = <T extends {name: string}>(obj: T) => {
  let uid = Math.floor(Math.random() * 100);
  return {...obj, uid};
}

let docOne = addUID({name: 'Hello', age: 30});
console.log(docOne); // { name: 'Hello', age: 30, uid: 54 }

// Example 3:
// interface with dynamic type
interface Resource<T> {
  uid: number,
  name: string,
  data: T,
}

// data type is string array
const docThree: Resource<string[]> = {
  uid: 100,
  name: 'Hello',
  data: ['aa', 'bb'],
}

// data type is number
const docFour: Resource<number> = {
  uid: 100,
  name: 'Hello',
  data: 5566,
}

// Example 4:
function simpleState<T>(initial: T): [() => T, (v: T) => void] {
  let val: T = initial;
  return [
    () => val,
    (v: T) => {
      val = v;
    }
  ]
}

// overriding inferred generic type, either number or null.
const [strGetter, strSetter] = simpleState<string | null>(null);
console.log(strGetter()); // null

strSetter('hi');
console.log(strGetter()); // hi

// Error: Argument of type 'number' is not assignable to parameter of type 'string'.ts(2345)
// strSetter(20);

strSetter(null);
console.log(strGetter()); // null
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


Another example of using `Unknown` type to handle api error response:

```js
const getErrorMessage = (error: unknown): string => {
  let message;
  if (error instanceof Error){
    message = error.message;
  }
  else if (error && typeof error === "object" && "message" in error){
    message = String(error.message)
  }
  else if (typeof error === "string){
    message = error;
  }
  else {
    message = "unknown error";
  }
}

export const sendEmail = async() => {
  try {
    await resend.emails.send({
      from: 'Contact form <admin@local.com>',
      to: 'test@local.com',
      subject: 'Message from contact form',
      html: '<p>Hello</p>'
    })
  } catch (error: unknown) {
    return {
      message: getErrorMessage(error)
    }
  }
}
```

Refs:
- https://juejin.cn/post/7018805943710253086
- https://www.tutorialsteacher.com/typescript
- https://www.youtube.com/watch?v=-bmNkTqvYfQ