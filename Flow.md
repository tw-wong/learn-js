# Flow
Static type checker for Javascript.

## Overview

* [Primitive Types](#primitive-types)
* [Maybe types](#maybe-types)
* [Optional object properties](#optional-object-properties)
* [Optional function parameters](#optional-function-parameters)
* [Function parameters with defaults](#function-parameters-with-defaults)
* [Literal Types](#literal-types)
* [Mixed Types](#mixed-types)

## Primitive Types

* Booleans

  ```javascript
  // @flow
  function acceptsBoolean(value: boolean) {
    // ...
  }
  ```

* Strings

  ```javascript
  // @flow
  function acceptsString(value: string) {
    // ...
  }
  ```

* Numbers

  ```javascript
  // @flow
  function acceptsNumber(value: number) {
    // ...
  }
  ```

* null
  ```javascript
  // @flow
  function acceptsNull(value: null) {
    // ...
  }

  acceptsNull(null);      // Works!
  acceptsNull(undefined); // Error!
  ```

* undefined (void in Flow types)

  ```javascript
  // @flow
  function acceptsUndefined(value: void) {
    // ...
  }

  acceptsUndefined(null);      // Error!
  acceptsUndefined(undefined); // Works!
  ```

## Maybe Types

  ```javascript
  // @flow
  function acceptsMaybeString(value: ?string) {
    // ...
  }

  acceptsMaybeString("bar");     // Works!
  acceptsMaybeString(undefined); // Works!
  acceptsMaybeString(null);      // Works!
  acceptsMaybeString();          // Works!
  ```

  * Maybe types are for places where a value is optional.
  * Adding a question mark in front of the type such as `?string` or `?number`.

## Optional object properties

  ```javascript
  // { propertyName?: string }

  // @flow
  function acceptsObject(value: { foo?: string }) {
    // ...
  }

  acceptsObject({ foo: "bar" });     // Works!
  acceptsObject({ foo: undefined }); // Works!
  acceptsObject({ foo: null });      // Error!
  acceptsObject({});                 // Works!
  acceptsObject();                   // Error!
  ```

  * Object types can have optional properties.
  * Adding a question mark after the property name.

  ## Optional function parameters

  ```javascript
  // @flow
  function acceptsOptionalString(value?: string) {
    // ...
  }

  acceptsOptionalString("bar");     // Works!
  acceptsOptionalString(undefined); // Works!
  acceptsOptionalString(null);      // Error!
  acceptsOptionalString();          // Works!
  ```

  * Optional parameters can either be `void` or omitted altogether.

  * But it cannot be `null`.

## Function parameters with defaults

```javascript
  // @flow
function acceptsOptionalString(value: string = "foo") {
  // ...
}

acceptsOptionalString("bar");     // Works!
acceptsOptionalString(undefined); // Works!
acceptsOptionalString(null);      // Error!
acceptsOptionalString();          // Works!
```

* Default parameters can also be `void` or omitted altogether.

* But they cannot be `null`.

## Literal Types

```javascript
// @flow
function acceptsTwo(value: 2) {
  // ...
}

acceptsTwo(2);   // Works!
// $ExpectError
acceptsTwo(3);   // Error!
// $ExpectError
acceptsTwo("2"); // Error!
```

* We can use Primitive value as literal type. Ex:

  * Booleans: like `true` or `false`.
  * Numbers: like `42` or `3.14`.
  * Strings: like `"foo"` or `"bar"`.

## Mixed Types

```javascript
function stringifyBasicValue(value: string | number) {
  return '' + value;
}
```

* We can define group of different possible types.

```javascript
function identity<T>(value: T): T {
  return value;
}
```

* Type based on another type (Generic Types).

* The return type will be the same as the type of whatever value is passed into (`value: T`) the function.
