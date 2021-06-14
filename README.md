# Wag! JavaScript Style Guide 

Other Style Guides
  > These style guides inherits from this main style guide. If a certain topic isn't available on these style guides it means that it is already covered on this main guide.

- [React](https://github.com/johnsantos-wag/wag-javascript-style-guide/tree/master/react)
- [TypeScript](https://github.com/johnsantos-wag/wag-javascript-style-guide/tree/master/typescript)

## Table of Contents

1. [Variables](#variables)
2. [Functions](#functions)
3. [Accessors](#accessors)
4. [Naming conventions](#naming-conventions)
5. [Modules](#modules)
6. [Testing](#testing)

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

- Use arrow function instead of function declaration

> This avoids function hoisted which can be confusing to track on

```js
// bad
function getDogs() {}

// good
const getDogs = () => {}
```

- Pass an object instead of one-by-one argument

> Passing a one-by-one argument to a function results in scalability problems. This avoids breaking the usage of existing function consumers

```js
// bad
const createDog = (name, breed, age) => {}
const createWalker = (name, age) => {}

// good
const createDog = (dog) => {}
const createDog = ({ name, breed, age }) => {}

const createWalker = (walker) => {}
const createWalker = ({ name, age }) => {}
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

- When retrieving or adding a non camelCase property, use a variable to do that

> This allows the need to disable linting. We usually encounter this when doing our integration with WagApi or wagcontent

```js
// bad
const payload = {
  /* eslint-disable */
  start_date: '1970-01-01',
};

payload.start_date

// good
const startDate = 'start_date';
const payload = {
  [startDate]: '1970-01-01',
};

payload[startDate]

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

## Iterators

- Prefer functional loops when iterating instead of imperative loops (`for loop`, `for in`, `while loop`, etc)

> This will prevent creating side-effects that is often the cause of bugs and this also provides better readability

```js
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {
  sum += num;
}
sum === 15;

// good
let sum = 0;
numbers.forEach((num) => {
  sum += num;
});
sum === 15;

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;

// bad
const increasedByOne = [];
for (let i = 0; i < numbers.length; i++) {
  increasedByOne.push(numbers[i] + 1);
}

// good
const increasedByOne = [];
numbers.forEach((num) => {
  increasedByOne.push(num + 1);
});

// best (keeping it functional)
const increasedByOne = numbers.map((num) => num + 1);
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

- Separate `import` statements or group by "type" and add whitespaces in between

> A type can be a third party package, utils path, assets path  

```js

// bad
import _ from 'lodash';
import { formatName } from './utils';
import moment from 'moment';
import dogIcon from '../assets/dog.svg';

// ok
import _ from 'lodash';
import moment from 'moment';
import { formatName } from './utils';
import dogIcon from '../assets/dog.svg';

// good
import _ from 'lodash';
import moment from 'moment';

import { formatName } from './utils';

import dogIcon from '../assets/dog.svg';
```

- One import per line

> This allows easy understanding when reviewing code (same with one prop per line)

```js
// bad
import { formatName, formatBirthday } from './utils'; 

// good
import {
  formatName,
  formatBirthday,
} from './utils';
```

## Testing

- Structure the test in two separate `describe` which creates a division between "happy paths" and "exception paths"

> Happy paths for normal user flows; exception paths for edge cases where the function may act abnormally. Separation of test types allow you to separate a normal case and an edge easily

```js
// bad
describe('my-test-name', () => {
  it('should ...', () => {
    ...
  });
  it('should ...', () => {
    ...
  });
});

// good
describe('my-test-name', () => {
  describe('happy paths', () => {
    it('should ...', () => {
      ...
    });
  });
  describe('exception paths', () => {
    it('should ...', () => {
      ...
    });
  });
});
```

- Use `it` instead of `test` when creating the test and follow the convetion of "it should..."

> `it` is much more easy to understand when creating test cases like you are just reading a normal sentence

```js
// bad
describe('my-test', () => {
  describe('happy paths', () => {
    test('should ...', () => {
      ...
    });
  });
  describe('exception paths', () => {
    test('should ...', () => {
      ...
    });
  });
});

// good
describe('my-test', () => {
  describe('happy paths', () => {
    it('should ...', () => {
      ...
    });
  });
  describe('exception paths', () => {
    it('should ...', () => {
      ...
    });
  });
});
```

- Query first the element before doing an assertion (arrange, act, assert guidelines)

> Arrange, act, assert guideline is based on the Microsoft model of writing test. It encourages you to (arrange) the data, elements and etc for your tests ahead of time, then you provide the actions (act) that the test will do. Sometimes the (act) part can be done along with the arrangement of the test. Then after that, do the assertions (assert).
> For more info, visit: https://docs.microsoft.com/en-us/visualstudio/test/unit-test-basics?view=vs-2019#write-your-tests

```js
// bad
describe('my-test', () => {
  describe('happy paths', () => {
    test('should ...', () => {
      expect(screen.getByTestId('my-element').toHaveTextContent('Hello World'))
    });
  });
  describe('exception paths', () => {
    test('should ...', () => {
      ...
    });
  });
});

// good
describe('my-test', () => {
  describe('happy paths', () => {
    it('should ...', () => {
      // arrange & act
      const expected = 'Hello World';
      const elem = screen.getByTestId('my-element');

      // assert
      expect(elem.toHaveTextContent(expected))
    });
  });
  describe('exception paths', () => {
    it('should ...', () => {
      ...
    });
  });
});

```