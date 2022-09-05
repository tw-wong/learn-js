# ES Modules

## Overview
* [Export](#export)
* [Import](#import)

## Export

```js
// export one by one
export function Test() {
  console.log(123);
}

export class Alligator {
   constructor() {
     // ...
   }
}

// Or you can export desired members in a single statement at the end of the module:
export { Test, Alligator };

// exporting with alias
export { Test as Debug, Alligator };


// exporting as default. ex: common.js
export default function(arr) {
  // sorting here
  console.log(arr.join())
}

```

## Import

```js
import { what, ever } from './other_module.js';

// import entire module as an object
import * as cal from './cal.js';

// use it as follows:
// cal.a;
// cal.b;
// cal.sum();


// import default export:
import common from './common.js';
common(['a', 'b', 'c']); // output: abc

// alias default export
import {default as print} from './common.js';
print(['a', 'b', 'c']); // output: abc
```