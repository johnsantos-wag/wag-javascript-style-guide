# Wag! JavaScript Style Guide 

Other Style Guides

- [React](https://github.com/johnsantos-wag/wag-javascript-styleguide/tree/master/react)

## Table of Contents

1. [Variables](#variables)
2. [Functions](#functions)
3. [Types](#types)
4. [Accessors](#accessors)
5. [Naming conventions](#naming-conventions)
6. [Modules](#modules)

## Variables:

- Prefer `const` over `let` or `var`

> You can use `let` if it's really needed but as much as possible use `const` as it provides better immutability. This removes the unintended mutation or side-effects. Sometimes restructuring or rethinking the approach is needed to adjust it

```js
// bad
var foo = 'foo';

// ok
let foo = 'foo';

// good
const foo = 'foo';

```

- Put the environment variables in a constant file instead of accessing directly

```js
// bad

// foo.js
function getUsers() {
  const baseUrl = process.env.BASE_URL || 'https://my-default-api.com';
  const url = process.env.BASE_URL + '/api/users';
  return fetch(url);
}

// good

// endpoints.constants.js
export const BASE_URL = process.env.BASE_URL || 'https://my-default-api.com';

// foo.js
function getUsers() {
  const url = BASE_URL + '/api/users';
  return fetch(url);
}

```

## Functions:

- Use function expression instead of function declaration

> This avoids function hoisted which can be confusing to track on

```js
// bad
function getDogs() {}

// good
const getDogs = () => {}
```

## Types:

- Use `type` when creating a typing definition
> both works but `type` is much shorter to work on

```ts
// bad 
interface Shape {}

// good
type Shape = {}
```

## Accessors:

- When accessing a value, use a guard style or an early-return approach

> This allows the code to exclude the error cases early on and provides a clear path to the intent

```js
// bad
const person = {
  age: 0,
};

if (person.age) { 
  ...
} else {
  throw new Error('Invalid age');
}

// good
const person = {
  age: 0,
};

if (!person.age) {
  throw new Error('Invalid age');  
}
```

## Naming conventions:

- Acronyms should be in PascalCase or camelCase (https://stackoverflow.com/questions/15526107/acronyms-in-camelcase)

> When using acronyms, use Pascal case or camel case for acronyms more than two characters long. 

```js
// bad 
const imageURL = '';

const sendSMS = () => {};

const systemIo = {};

export const apiKey = 'qwerty'; // constant

// good
const imageUrl = '';

const sendSms = () => {};

const systemIO = {}; // if the acrnonym consist of two letters, uppercase

export const API_KEY = 'qwerty'; //constant
```


## Modules 

- Use `import`/`export` over `require`

```js
// bad
const fs = require('fs');
const { writeFile } = require('fs');

// good
import { writeFile } from 'fs';
```

- Use named export when exporting 

```js

// bad
const formatUsername = () => {};
export default formatUsername;

// good
export const formatUsername = () => {};
```
