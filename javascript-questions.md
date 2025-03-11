# JavaScript Interview Questions

## Core JavaScript Concepts

### 1. What is a closure in JavaScript?

<details>
<summary>View Answer</summary>

A closure is a function that has access to its own scope, the outer function's scope, and the global scope, even after the outer function has finished executing. It "closes over" the variables from its parent function's scope.

**Example:**

```javascript
function createCounter() {
  let count = 0; // This variable is "closed over"

  return function () {
    count += 1;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

</details>

### 2. Explain the event loop in JavaScript.

<details>
<summary>View Answer</summary>

The event loop is a mechanism that allows JavaScript to perform non-blocking operations despite being single-threaded. It works by:

1. Executing code in the call stack
2. Moving asynchronous tasks (like callbacks, promises, and event handlers) to the task queue or microtask queue when they're ready
3. When the call stack is empty, moving tasks from the queues to the call stack

The event loop continuously checks if the call stack is empty, and if it is, it takes the first task from the queue and pushes it to the call stack for execution.

Microtasks (from Promises) have priority over regular tasks (from setTimeout, events, etc.).

</details>

### 3. What's the difference between `null` and `undefined`?

<details>
<summary>View Answer</summary>

- `undefined` means a variable has been declared but has not been assigned a value
- `null` is an assignment value that represents no value or no object
- `undefined` is a type itself, while `null` is an object
- `typeof undefined` is 'undefined', whereas `typeof null` is 'object' (this is considered a bug in JavaScript)
</details>

### 4. Explain hoisting in JavaScript.

<details>
<summary>View Answer</summary>

Hoisting is JavaScript's default behavior of moving declarations to the top of the current scope during the compilation phase.

- Variable declarations (`var`) are hoisted and initialized with `undefined`
- `let` and `const` declarations are hoisted but not initialized (Temporal Dead Zone)
- Function declarations are hoisted completely with their implementation
- Function expressions are not hoisted with their implementation

```javascript
// This works because of hoisting
sayHello();

function sayHello() {
  console.log("Hello");
}

// This doesn't work - TypeError
sayGoodbye();

var sayGoodbye = function () {
  console.log("Goodbye");
};
```

</details>

### 5. What is the difference between `==` and `===` operators?

<details>
<summary>View Answer</summary>

- `==` (loose equality) compares values after type conversion
- `===` (strict equality) compares both value and type without conversion
- `===` is generally preferred as it avoids unexpected type coercion issues
</details>

## Asynchronous JavaScript

### 1. Explain Promises and their benefits over callbacks.

<details>
<summary>View Answer</summary>

Promises are objects representing the eventual completion or failure of an asynchronous operation. Benefits include:

- Avoiding callback hell (deeply nested callbacks)
- Better error handling with `.catch()`
- Chaining with `.then()`
- Composability with `Promise.all()`, `Promise.race()`, etc.
- A more readable flow of asynchronous operations
</details>

### 2. What is async/await and how does it improve asynchronous code?

<details>
<summary>View Answer</summary>

`async/await` is syntactic sugar on top of Promises that allows asynchronous code to be written in a more synchronous style.

- An `async` function always returns a Promise
- The `await` keyword pauses execution until the Promise resolves
- It makes asynchronous code more readable and maintainable
- It simplifies error handling using try/catch blocks

```javascript
// Promise-based approach
function getUserData() {
  return fetch("/api/user")
    .then((response) => response.json())
    .then((data) => console.log(data))
    .catch((error) => console.error(error));
}

// async/await approach
async function getUserData() {
  try {
    const response = await fetch("/api/user");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

</details>

### 3. What is the difference between microtasks and macrotasks in the event loop?

<details>
<summary>View Answer</summary>

- **Macrotasks**: setTimeout, setInterval, setImmediate, I/O operations, UI rendering
- **Microtasks**: Promise callbacks, queueMicrotask, MutationObserver callbacks

The event loop prioritizes microtasks over macrotasks. After each macrotask, the event loop will empty the entire microtask queue before moving on to the next macrotask.

</details>

## ES6+ Features

### 1. Explain destructuring in ES6.

<details>
<summary>View Answer</summary>

Destructuring is a JavaScript expression that allows extracting values from arrays or properties from objects into distinct variables.

```javascript
// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first, second, rest); // 1, 2, [3, 4, 5]

// Object destructuring
const { name, age, job = "Developer" } = { name: "John", age: 30 };
console.log(name, age, job); // 'John', 30, 'Developer'
```

</details>

### 2. What are arrow functions and how do they differ from regular functions?

<details>
<summary>View Answer</summary>

Arrow functions are a more concise syntax for writing functions in ES6. Key differences:

- Shorter syntax: `(params) => expression` or `(params) => { statements }`
- No `this` binding: `this` is lexically inherited from the surrounding code
- No `arguments` object
- Cannot be used as constructors (no `new` keyword)
- No `super` or `new.target`
- Implicit return for single-expression functions
</details>

### 3. Explain the spread operator and rest parameters.

<details>
<summary>View Answer</summary>

- **Spread operator** (`...`) expands an iterable (array, string) into individual elements

```javascript
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }
```

- **Rest parameters** (`...`) collect multiple elements into an array

```javascript
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}

sum(1, 2, 3, 4); // 10
```

</details>

## Advanced Concepts

### 1. Explain the concept of event delegation.

<details>
<summary>View Answer</summary>

Event delegation is a technique where instead of attaching an event listener to each individual similar element, the event listener is attached to a parent element. Events that occur on the children "bubble up" to the parent. This is more efficient for:

- Dynamically created elements
- Large numbers of similar elements
- Reduced memory usage

```javascript
// Instead of this:
document.querySelectorAll("li").forEach((item) => {
  item.addEventListener("click", handleClick);
});

// Use this:
document.querySelector("ul").addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    handleClick(event);
  }
});
```

</details>

### 2. What is memoization and how would you implement it?

<details>
<summary>View Answer</summary>

Memoization is an optimization technique that stores the results of expensive function calls and returns the cached result when the same inputs occur again.

```javascript
function memoize(fn) {
  const cache = {};

  return function (...args) {
    const key = JSON.stringify(args);
    if (cache[key]) {
      return cache[key];
    }

    const result = fn.apply(this, args);
    cache[key] = result;
    return result;
  };
}

// Usage
const expensiveFunction = memoize((n) => {
  console.log("Computing...");
  return n * n;
});

console.log(expensiveFunction(10)); // Computing... 100
console.log(expensiveFunction(10)); // 100 (from cache)
```

</details>

### 3. What is the purpose of the `use strict` directive?

<details>
<summary>View Answer</summary>

`"use strict"` is a literal expression that enables strict mode in JavaScript. It helps by:

- Preventing the use of undeclared variables
- Making assignments to non-writable properties throw errors
- Making attempts to delete undeletable properties throw errors
- Requiring unique parameter names in functions
- Making `this` undefined in functions called without an object context
- Forbidding octal syntax and the `with` statement
- Helping code run faster by enabling optimizations
</details>

### 4. Explain JavaScript modules (ES modules) and their benefits.

<details>
<summary>View Answer</summary>

ES modules are a standard format to package JavaScript code for reuse. Features include:

- Each module has its own scope (no global namespace pollution)
- Static structure (imports/exports can't be changed at runtime)
- Default and named exports/imports
- Top-level `await` (in modern browsers)
- Better dependency management
- Tree-shaking capability for optimized bundling

```javascript
// Exporting
export const PI = 3.14159;
export function circleArea(radius) {
  return PI * radius * radius;
}
export default class Circle {
  /* ... */
}

// Importing
import Circle, { PI, circleArea } from "./circle.js";
```

</details>
