# TypeScript Interview Questions

## TypeScript Fundamentals

### 1. What is TypeScript and what are its advantages over JavaScript?

**Answer:** TypeScript is a strongly-typed, object-oriented, compiled language that is a superset of JavaScript. Advantages include:

- Static type checking at compile time
- Better IDE support with autocompletion and intellisense
- Early detection of errors
- Explicit interfaces and type definitions
- Better code organization with modules and namespaces
- Support for the latest ECMAScript features
- Improved maintainability for large codebases

### 2. Explain the difference between `interface` and `type` in TypeScript.

**Answer:** Both `interface` and `type` are used for defining custom types, but they have some differences:

**Interfaces:**

- Can be extended using `extends` keyword
- Can be implemented by classes
- Can be merged when declared multiple times (declaration merging)
- Primarily used for object shapes

**Types:**

- Can create union and intersection types
- Can create mapped types and conditional types
- Can create tuple and array types more easily
- Can create utility types
- Can represent primitives, unions, and more complex types

```typescript
// Interface
interface User {
  name: string;
  age: number;
}

interface Employee extends User {
  employeeId: number;
}

// Type
type Point = {
  x: number;
  y: number;
};

type ID = string | number;
type UserOrPoint = User | Point;
```

### 3. What are TypeScript generics and why are they useful?

**Answer:** Generics allow you to create reusable components that work with a variety of types rather than a single one. They provide a way to make components work with any data type and not restrict to one.

Benefits include:

- Type safety while maintaining flexibility
- Code reusability
- Reducing the need for type casting
- Self-documenting code that conveys intent

```typescript
// Generic function
function identity<T>(arg: T): T {
  return arg;
}

// Usage
let output1 = identity<string>("myString"); // type of output will be 'string'
let output2 = identity(123); // type argument inferred as number

// Generic interface
interface Collection<T> {
  add(item: T): void;
  get(index: number): T;
  size(): number;
}

// Generic class
class List<T> implements Collection<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  get(index: number): T {
    return this.items[index];
  }

  size(): number {
    return this.items.length;
  }
}
```

### 4. What are the different access modifiers in TypeScript?

**Answer:** TypeScript has three access modifiers:

- `public`: (default) Accessible from anywhere
- `private`: Accessible only within the class
- `protected`: Accessible within the class and its subclasses

There's also:

- `readonly`: Properties marked as readonly can only be assigned during initialization
- TypeScript 4.0+ introduces `#` private fields (ECMAScript private fields)

```typescript
class Employee {
  public name: string;
  private ssn: string;
  protected department: string;
  readonly employeeId: number;
  #secretKey: string; // ECMAScript private field

  constructor(name: string, ssn: string, department: string, id: number) {
    this.name = name;
    this.ssn = ssn;
    this.department = department;
    this.employeeId = id;
    this.#secretKey = "generated-secret";
  }
}
```

### 5. Explain the `any`, `unknown`, and `never` types in TypeScript.

**Answer:**

- `any`: Represents any JavaScript value without type-checking. It effectively opts out of type checking.
- `unknown`: Similar to `any` but safer, as it requires type checking or assertion before performing operations on the values.
- `never`: Represents values that never occur. Used for functions that never return (always throw or have infinite loops) or as the type of a variable that can never have a value.

```typescript
// any
let anyValue: any = 42;
anyValue.toString(); // No error

// unknown
let unknownValue: unknown = 42;
// unknownValue.toString();  // Error: Object is of type 'unknown'
if (typeof unknownValue === "number") {
  unknownValue.toFixed(); // OK, type narrowing applied
}

// never
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

## Advanced TypeScript Concepts

### 1. What are Union and Intersection types?

**Answer:**

- **Union types** (`|`) represent values that could be one of several types. A value has a union type when it could be one of the specified types.
- **Intersection types** (`&`) combine multiple types into one, allowing you to add together existing types to get a single type with all features.

```typescript
// Union type
type StringOrNumber = string | number;
let value: StringOrNumber = "hello";
value = 42; // Also valid

// Intersection type
type Employee = {
  id: number;
  name: string;
};

type Manager = {
  employees: Employee[];
  department: string;
};

type ManagerWithEmployeeInfo = Employee & Manager;

const manager: ManagerWithEmployeeInfo = {
  id: 1,
  name: "John",
  employees: [],
  department: "Engineering",
};
```

### 2. Explain Mapped Types and provide examples.

**Answer:** Mapped types allow you to create new types based on old ones by transforming properties. They are built on the syntax for index signatures.

```typescript
// Basic mapped type
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};

// Usage
interface Person {
  name: string;
  age: number;
}

const readonlyPerson: Readonly<Person> = {
  name: "John",
  age: 30,
};

// readonlyPerson.name = "Jane"; // Error: Cannot assign to 'name' because it is a read-only property

// More complex mapped type with modifiers
type Optional<T> = {
  [P in keyof T]?: T[P];
};

type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};
```

### 3. What are Conditional Types in TypeScript?

**Answer:** Conditional types select one of two possible types based on a condition. They help to model the relationship between types in a more expressive way.

```typescript
type TypeName<T> = T extends string
  ? "string"
  : T extends number
  ? "number"
  : T extends boolean
  ? "boolean"
  : T extends undefined
  ? "undefined"
  : T extends Function
  ? "function"
  : "object";

type T0 = TypeName<string>; // "string"
type T1 = TypeName<"a">; // "string"
type T2 = TypeName<true>; // "boolean"
type T3 = TypeName<() => void>; // "function"
type T4 = TypeName<string[]>; // "object"

// Distributive conditional types
type NonNullable<T> = T extends null | undefined ? never : T;
type T5 = NonNullable<string | number | undefined>; // string | number
```

### 4. Explain TypeScript Decorators.

**Answer:** Decorators are a special kind of declaration that can be attached to classes, methods, accessors, properties, or parameters. They are functions that can modify the behavior of the decorated item.

```typescript
// Method Decorator
function log(target: any, key: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${key} with`, args);
    return originalMethod.apply(this, args);
  };

  return descriptor;
}

// Class Decorator
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

// Usage
@sealed
class Person {
  constructor(public name: string) {}

  @log
  greet(message: string) {
    return `${this.name} says: ${message}`;
  }
}
```

### 5. What are some common TypeScript utility types and when would you use them?

**Answer:** TypeScript provides several utility types to help with common type transformations:

- `Partial<T>`: Makes all properties in T optional
- `Required<T>`: Makes all properties in T required
- `Readonly<T>`: Makes all properties in T readonly
- `Record<K, T>`: Creates a type with properties of type K and values of type T
- `Pick<T, K>`: Creates a type by picking the set of properties K from T
- `Omit<T, K>`: Creates a type by omitting the set of properties K from T
- `Exclude<T, U>`: Excludes from T those types that are assignable to U
- `Extract<T, U>`: Extracts from T those types that are assignable to U
- `NonNullable<T>`: Removes null and undefined from T
- `ReturnType<T>`: Obtains the return type of a function type
- `Parameters<T>`: Obtains the parameter types of a function type

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  active: boolean;
}

// Make all fields optional
type PartialUser = Partial<User>;
// { id?: number; name?: string; email?: string; active?: boolean; }

// Pick specific fields
type UserCredentials = Pick<User, "email" | "id">;
// { email: string; id: number; }

// Omit specific fields
type PublicUser = Omit<User, "email">;
// { id: number; name: string; active: boolean; }

// Create a map of users
type UserMap = Record<string, User>;
// { [key: string]: User }
```

## TypeScript Coding Challenges

### Challenge 1: Implement a Generic Stack

**Problem:** Implement a generic stack data structure with `push`, `pop`, `peek`, and `isEmpty` methods.

**Solution:**

```typescript
class Stack<T> {
  private items: T[] = [];

  push(item: T): void {
    this.items.push(item);
  }

  pop(): T | undefined {
    return this.items.pop();
  }

  peek(): T | undefined {
    return this.items[this.items.length - 1];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }

  size(): number {
    return this.items.length;
  }
}

// Usage
const numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);
console.log(numberStack.peek()); // 2
console.log(numberStack.pop()); // 2
console.log(numberStack.size()); // 1
```

### Challenge 2: Type-Safe Event Emitter

**Problem:** Create a type-safe event emitter that ensures event handlers receive the correct argument types based on the event name.

**Solution:**

```typescript
type EventMap = {
  [eventName: string]: any;
};

class TypedEventEmitter<T extends EventMap> {
  private listeners: { [K in keyof T]?: Array<(data: T[K]) => void> } = {};

  on<K extends keyof T>(event: K, listener: (data: T[K]) => void): void {
    if (!this.listeners[event]) {
      this.listeners[event] = [];
    }
    this.listeners[event]!.push(listener);
  }

  emit<K extends keyof T>(event: K, data: T[K]): void {
    const eventListeners = this.listeners[event];
    if (eventListeners) {
      eventListeners.forEach((listener) => listener(data));
    }
  }

  off<K extends keyof T>(event: K, listener: (data: T[K]) => void): void {
    const eventListeners = this.listeners[event];
    if (eventListeners) {
      this.listeners[event] = eventListeners.filter((l) => l !== listener);
    }
  }
}

// Usage
interface AppEvents {
  userLoggedIn: { userId: string; timestamp: number };
  buttonClicked: { buttonId: string; x: number; y: number };
  formSubmitted: { formData: Record<string, string> };
}

const eventEmitter = new TypedEventEmitter<AppEvents>();

eventEmitter.on("userLoggedIn", (data) => {
  console.log(`User ${data.userId} logged in at ${data.timestamp}`);
});

eventEmitter.emit("userLoggedIn", {
  userId: "user-123",
  timestamp: Date.now(),
});

// Type error: wrong event parameter type
// eventEmitter.emit('userLoggedIn', { buttonId: 'btn-1' });
```

### Challenge 3: Implementing a Type-Safe Function Overloads

**Problem:** Create a function that can properly handle different parameter types with appropriate return types.

**Solution:**

```typescript
// Function overloads
function process(input: number): number;
function process(input: string): string;
function process(input: boolean): boolean;
function process(input: number[]): number;

// Implementation
function process(
  input: number | string | boolean | number[]
): number | string | boolean {
  if (typeof input === "number") {
    return input * 2;
  } else if (typeof input === "string") {
    return input.toUpperCase();
  } else if (typeof input === "boolean") {
    return !input;
  } else if (Array.isArray(input)) {
    return input.reduce((sum, num) => sum + num, 0);
  }

  // This should never happen due to TypeScript's type checking,
  // but necessary for the implementation
  throw new Error("Invalid input type");
}

// Usage
const result1 = process(10); // 20
const result2 = process("hello"); // 'HELLO'
const result3 = process(true); // false
const result4 = process([1, 2, 3]); // 6

// Type error
// const result5 = process({});
```

### Challenge 4: Advanced Type Manipulation

**Problem:** Write a type utility that extracts the exact return type from a function, including promises.

**Solution:**

```typescript
// Unwrap promise types
type Awaited<T> = T extends Promise<infer U> ? Awaited<U> : T;

// Get exact return type, including promises
type ExactReturnType<T extends (...args: any) => any> = Awaited<ReturnType<T>>;

// Examples
async function fetchUser() {
  return { id: 1, name: "John" };
}

async function fetchPosts() {
  return Promise.resolve([
    { id: 1, title: "Post 1" },
    { id: 2, title: "Post 2" },
  ]);
}

function getNow() {
  return new Date();
}

// Results
type User = ExactReturnType<typeof fetchUser>; // { id: number; name: string }
type Posts = ExactReturnType<typeof fetchPosts>; // { id: number; title: string }[]
type Now = ExactReturnType<typeof getNow>; // Date
```
