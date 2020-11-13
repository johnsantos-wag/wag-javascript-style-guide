# Wag! React Style Guide 

## Table of Contents

1. [Functions](#functions)
2. [Types](#types)
3. [Naming conventions](#naming-conventions)
4. [Modules](#modules)


## Functions:

- Use function expression instead of function declaration

> This avoids function hoisted which can be confusing to track on

```jsx
// bad
function DogProfile() {}

// good
const DogProfile = () => {}
```

## Types:

- Use `type` when creating a typing definition
> Both works but `type` is much shorter to work on

```ts
// bad 
interface Shape {}

// good
type Shape = {}
```

## Naming conventions:

- Naming the rest of the props as <b> rest </b>

```js
// bad
const Demo = ({
  name,
  value,
  ...props
}) => {}

// good
const Demo = ({
  name,
  value,
  ...rest
}) => {}

```

- Component prop declaration should be the same component name plus the "Props" literal string

> Easier to reuse the props declaration when doing component compositions

```js
// bad 
export type Props = {}
export const Demo = (props: Props) => {}

// good
export type DemoProps = {}
export const Demo = (props: DemoProps) => {}
```

## Alignment

- Organizing component props (One prop per line)

```jsx
// bad
<Button disabled onClick={handleOnClick} />

// good
<Button
  disabled
  onClick={handleOnClick}
/>
```

## Quotes

- Use double quotes ("") for component props and single quote ('') for the rest

```jsx
// bad
<input
  id='my-input'
  value='hello world'
/>

//good
<input 
  id="my-input"
  value="hello world"
/>
```

## Modules 

- Use `import`/`export` over `require`

```jsx
// bad
const DogProfileComponent = require('DogProfileComponent');
const { DogProfileComponent } = require('DogProfileComponent');

// good
import { DogProfileComponent } from 'DogProfileComponent';
```

- Use named export when exporting instead of default export 

```jsx

// bad
const DogProfileComponent = () => {};
export default DogProfileComponent;

// good
export const DogProfileComponent = () => {};
```
