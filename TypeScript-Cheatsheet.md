[back to overview](/../..)
Looking for [JavaScript](../master/JavaScript-Cheatsheet.md)?

# TypeScript Cheatsheet<!-- omit in toc -->


# Concepts
- Provide Static Type Checking to JavaScript
- Types as Sets - In Typescript, a type is a set of types (i.e. `string | number`)
- Erased Structural Types - if an object satisfies an interface, you can use it as that interface (even if there is no declarative relationship between the two) - "duck typing"
- Types disappear at runtime - entire type system goes away in transpile step back to js
-

# Setup

- `npm install -g typescript` - installs tsc, the Typescript compiler
- `tsc XXX.ts` - outputs typescript errors to the console, and `XXX.js` in the directory
- `tsc --target es2015 X` - compile with target javascript version es2015


## Compile Options
- `strict` - toggle all options on
- `noImplicitAny` - if false (default unless `strict`) anywhere you don't define a type and typescript can't infer it for us, typescript will use `any`. If true, that will generate a compile time error instead
- `strictNullChecks` - if false, `null` and `undefined` can be assigned to any other type. If true, must explictly mark types nullable.

# Types

- Primitives: `string`, `number`, `boolean`
- Arrays: `type[]` or `Array<type>`
- `any`: ignore all typechecking on this type
- `object`: any value with properties
- Explicit type declaration: `[let/const/var] myVar: <type> = <value>;`
- Implicit type declaration: `[let/const/var] myVar = <value>;` - if `<type>` is inferrable from the type of `<value>` then its not necessary to explicitly specify it

## Objects
- `obj: { first: string; last?: string }`: note optional `last` - returns `undefined` if not set, rather than a runtime error

## Defining
- Anonymously: `{ name: string; age: number }`
- Type Alias:  `type Person = { name: string; age: number; };`
- Interface:   `interface Person { name: string; age: number; }`

## Inheritance

### Extends
Used with interfaces. Multiple inheritance allowed.
```javascript
interface Point { x: number; y: number; }
interface zPoint extends Point { z: number; }
```

### Intersection Types
Used with types or interfaces.
```javascript
type Point = { x:number; y:number; };
type zPoint = Point & { z: number };
```

### Differences
- You can reopen an interface to add properties, but not a type
- Property name conflicts:
  - With extends, you get an error
  - With intersection, you get both properties

## Property Modifiers
- Optional Properties: add `?` to the end of the name - `number?` == `number | undefined`
- `readonly`: can't be written to during type-checking

### Index Signatures
- property type is either `string` or `number`, and all possible values of object, when indexed by that type, will conform to the declared type
```javascript
interface StringArray {
  [index: number]: string;
}
const myArray: StringArray = getStringArray();
const secondItem = myArray[1]; // secondItem is a string
```
- all object properties must match return type
```javascript
interface NumberDictionary {
  [index: string]: number;

  length: number; // ok
  name: string;
// Property 'name' of type 'string' is not assignable to 'string' index type 'number'.
}

interface NumberOrStringDictionary {
  [index: string]: number | string;
  length: number; // ok, length is a number
  name: string; // ok, name is a string
}
```
- `readonly` index signatures prevent assignment to indices
```javascript
interface ReadonlyStringArray {
  readonly [index: number]: string;
}

let myArray: ReadonlyStringArray = getReadOnlyStringArray();
myArray[2] = "Mallory";
// Index signature in type 'ReadonlyStringArray' only permits reading.
```

## Generic Objects
```javascript
interface Box<Type> {
  contents: Type;
}
// OR
type Box<Type> = {
  contents: Type;
}

let box: Box<string>;
```

## Array Types
- `type[]` is an alias for `Array<type>`
- `readonly type[]` is an alias for `ReadonlyArray<type>`
  - Note: no constructor, use `const roArray: ReadonlyArray<string> = ["red", "green"];`

## Tuple Types
- Sort of `Array` that knows exactly how many elements and their types
```javascript
type StringNumberPair = [string, number];
fucntion fn(pair: StringNumberPair) {
  const a = pair[0]; // string
  const b = pair[1]; // number
  const c = pair[2]; // index out of range
  const [d, e] = pair; // d == a, e == b
}
```
- Length of tuple types are literals.
- You can have optional properties: `type Either2dOr3d = [number, number, number?];`
  - length of `Either2dOr3d` is `2 | 3`

## Type Assertions
- Casting
- `const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;` OR `const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");`

## Literal Types
- a specific number or string or boolean value can be referred to as a type
- `function printText(s: string, alignment: "left" | "right" | "center") {`

## Non-null Assertion Operator (Postfix !)
- `!` after any expression is a type assertion that the value isn’t null or undefined:
```javascript
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

## Union Types
- `id: number | string`: id responds to number or string messages

### Narrowing
- TypeScript will only allow an operation if it is valid for every member of the union
- If you have a value of a union type, you probably need to narrow before using it
```javascript
function printId(id: number | string) {
  if (typeof id === "string") {
    // In this branch, id is of type 'string'
    console.log(id.toUpperCase());
  } else {
    // Here, id is of type 'number'
    console.log(id);
  }
}
```

#### `never` Type
If you narrow out all possibilities, resulting type is `never`. You can use this for exhaustiveness checking:
```javascript
type Shape = Circle | Square | Triangle;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
//Type 'Triangle' is not assignable to type 'never'.
      return _exhaustiveCheck;
  }
}
```


# Functions
- `function f(param: <type>): <type> { ... }`
- Anonymous functions have contextual typing:
```javascript
// No type annotations here, but TypeScript can spot the bug
const names = ["Alice", "Bob", "Eve"];

// Contextual typing for function
names.forEach(function (s) {
  console.log(s.toUppercase());
// Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});

// Contextual typing also applies to arrow functions
names.forEach((s) => {
  console.log(s.toUppercase());
// Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});
```

## Function Type Expressions
```javascript
function greeter(fn: (a: string) => void) {
  fn("Hello, World");
}

function printToConsole(s: string) {
  console.log(s);
}

greeter(printToConsole);
```
Or you can use a type alias:
```javascript
type GreetFunction = (a: string) => void;
function greeter(fn: GreetFunction) {
  // ...
}
```

## Functions with Properties
```javascript
type DescribableFunction = {
  description: string;
  (someArg: number): boolean;
};
function doSomething(fn: DescribableFunction) {
  console.log(fn.description + " returned " + fn(6));
}
```

## Construct Signatures
```javascript
type SomeConstructor = {
  new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
  return new ctor("hello");
}

interface CallOrConstruct {
  new (s: string): Date;
  (n?: number): number;
}
```

## Generic Functions
```javascript
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}

// s is of type 'string'
const s = firstElement(["a", "b", "c"]);
// n is of type 'number'
const n = firstElement([1, 2, 3]);
// u is of type undefined
const u = firstElement([]);
```

You can specify the type instead of inferring it if necessary:
```javascript
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}
const arr = combine([1, 2, 3], ["hello"]);
//Type 'string' is not assignable to type 'number'.
const arr = combine<string | number>([1, 2, 3], ["hello"]);
```

### Constraints
```javascript
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}

// longerArray is of type 'number[]'
const longerArray = longest([1, 2], [1, 2, 3]);
// longerString is of type 'alice' | 'bob'
const longerString = longest("alice", "bob");
// Error! Numbers don't have a 'length' property
const notOK = longest(10, 100);
// Argument of type 'number' is not assignable to parameter of type '{ length: number; }'.
```

## Optional Parameters
- `number?` == `number | undefined`
- `function f(x = 10) {...}`: x = 10 if not provided (or undefined is provided)
- When writing a function type for a callback, never write an optional parameter unless you intend to call the function without passing that argument:
```javascript
function myForEach(arr: any[], callback: (arg: any, index?: number) => void) {
  for (let i = 0; i < arr.length; i++) {
    callback(arr[i], i);
  }
}

myForEach([1, 2, 3], (a, i) => {
  console.log(i.toFixed());
// Object is possibly 'undefined'.
});
```

## Function Overloads
- write two or more overload signatures (callable) and then a function implementation with a combined signature (not callable)
```javascript
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date { //can't call this signature alone
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}
const d1 = makeDate(12345678);
const d2 = makeDate(5, 5, 5);
const d3 = makeDate(1, 3);
// No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.
```

## Rest Parameters/Arguments

Rest Parameters - take unbounded number of arguments
- Implicit type annotation is `any[]` instead of `any`.
- Explicit type annotation must be of the form `Array<T>` or `T[]`, or a tuple type
```javascript
function multiply(n: number, ...m: number[]) {
  return m.map((x) => n * x);
}
// 'a' gets value [10, 20, 30, 40]
const a = multiply(10, 1, 2, 3, 4);
```

Rest Arguments - provide unbounded number of arguments
```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
arr1.push(...arr2);

// Inferred type is number[] -- "an array with zero or more numbers",
// not specifically two numbers
const args = [8, 5];
const angle = Math.atan2(...args);
// A spread argument must either have a tuple type or be passed to a rest parameter.

// Inferred as 2-length tuple
const args = [8, 5] as const;
// OK
const angle = Math.atan2(...args);
```

## Parameter Destructuring
```javascript
function sum({ a, b, c }: { a: number; b: number; c: number }) {
  console.log(a + b + c);
}
// OR
type ABC = { a: number; b: number; c: number };
function sum({ a, b, c }: ABC) {
  console.log(a + b + c);
}
```
