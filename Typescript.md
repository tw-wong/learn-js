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
* [Array constraint item type](#array-constraint-item-type)

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

## Array constraint item type
```js
// array and must contains 2 items
let x: [string, number];

// x = ['hello', 'world']; // Error
// x = ['hello', 10, 10];  // Error
// x = [10, 'hello'];      // Error
// x = [123, 'hello'];     // Error
x = ['hello', 123];        // OK

console.log(x);
```

Refs:
- https://juejin.cn/post/7018805943710253086