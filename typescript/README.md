# Wag! TypeScript Style Guide 

## Table of Contents

1. [Variables](#variables)
2. [Types](#types)

## Variables:

- Prefer to use `enum` instead of literal strings for constants that are in a grouped context

```ts
// bad
const WALK_STATUS_ERROR = 0;
const WALK_STATUS_COMPLETED = 1;

// ok
const WALK_STATUS = {
  ERROR: 0,
  COMPLETED: 1,
};

// good
enum WalkStatus {
  Error = 0,
  Completed = 1,
}

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
