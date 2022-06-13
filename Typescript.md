# Typescript

## Overview
* [Setup](#setup)
* [Types](#types)
* [Array](#array)
* [Interface](#interface)
* [Interface as function type](#interface-as-function-type)
* [Interface for array type](#interface-for-array-type)
* [Function](#function)

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

```


Refs:
- https://juejin.cn/post/7018805943710253086