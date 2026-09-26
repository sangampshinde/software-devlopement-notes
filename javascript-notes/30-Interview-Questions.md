## 1.What are the differences between `var`, `let`, and `const` in JavaScript?

Here's a comprehensive breakdown of the differences between var, let, and const in JavaScript:

1. Scope

- `var` =>	Function-scoped (or global if declared outside a function)
- `let` =>	Block-scoped (limited to {} block)
- `const` =>  Block-scoped (limited to {} block)


example:

```
function example() {
  if (true) {
    var a = 1;
    let b = 2;
    const c = 3;
  }
  console.log(a); // 1 (var leaks out of the block)
  console.log(b); // ReferenceError
  console.log(c); // ReferenceError
}

```

2. Hoisting

- `var` Hoisted and initialized with `undefined`.
- `let` and `const` Hoisted but not initialized — they're in a "Temporal Dead Zone" (TDZ) until the declaration is reached.

example:

```
console.log(a); // undefined
var a = 1;

console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 2;

```
3. Re-declaration

- `var` - ✅ Yes
- `let` and `const` - ❌ No

example:

```
var x = 1;
var x = 2; // OK

let y = 1;
let y = 2; // SyntaxError

```

4. Reassignment

`var`	✅ Yes
`let`	✅ Yes
`const`	❌ No (for primitive values)

example:

```
const z = 1;
z = 2; // TypeError: Assignment to constant variable

// Note: const prevents reassignment, not mutation. Object properties can still be changed.

const obj = { name: "Alice" };
obj.name = "Bob"; // ✅ Allowed
obj = {};         // ❌ TypeError


```

## 2 Explain the concept of hoisting in JavaScript ?

`Hoisting` is JavaScript's default behavior of moving declarations to the top of their scope before code execution.

Two Phases of Execution

When JavaScript runs, it processes code in two passes:

1. `Creation Phase (Compilation)` — The engine scans the code and registers all declarations in memory.

2. `Execution Phase` — Code runs line by line, and assignments happen.

Hoisting is what happens during the `creation phase`.

example:

1. Function Declarations Are Fully Hoisted

- Function declarations are hoisted with their entire definition, so you can call them before they appear in the code.

example:

```
sayHello(); // ✅ Works

function sayHello() {
  console.log("Hello!");
}

```

2. `var` Is Hoisted and Initialized with `undefined`

example:

```
console.log(x); // undefined (not an error!)
var x = 5;
console.log(x); // 5

```

3. `let` and `const` Are Hoisted but in the Temporal Dead Zone (TDZ)

- `let` and `const` are hoisted, but they are not initialized. Accessing them before their declaration throws a `ReferenceError`.
- The period between the start of the scope and the declaration is called the Temporal Dead Zone (TDZ).

example:
```
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;

console.log(z); // ReferenceError: Cannot access 'z' before initialization
const z = 20;
```

4. Function Expressions Are NOT Fully Hoisted

- if a function is assigned to a variable, only the variable declaration is hoisted — not the function body.

example:

```
greet(); // ❌ TypeError: greet is not a function

var greet = function () {
  console.log("Hi!");
};


greet(); // ❌ ReferenceError (TDZ)

const greet = function () {
  console.log("Hi!");
};


```

5. Hoisting Is Per-Scope, Not Global

- Each function and block has its own hoisting behavior.
javascript

```
var a = "outer";

function test() {
  console.log(a); // undefined (local `a` is hoisted over the outer one)
  var a = "inner";
  console.log(a); // "inner"
}

test();

```

## 3. Difference Between == and === in JavaScript ?

- `==`  — Loose equality (compares value after type coercion)
- `===` — Strict equality (compares value AND type, no coercion)

## 2. Common Examples

| Expression | `==` | `===` |
|---|---:|---:|
| `5 == "5"` | `true` | `false` |
| `0 == false` | `true` | `false` |
| `"" == false` | `true` | `false` |
| `null == undefined` | `true` | `false` |
| `null === undefined` | `false` | `false` |
| `NaN == NaN` | `false` | `false` |
| `[] == false` | `true` | `false` |
| `[1] == 1` | `true` | `false` |
| `"1" == true` | `true` | `false` |
| `{} == {}` | `false` | `false` |


## 4 What are primitive and non-primitive data types in JavaScript?

- JavaScript data types fall into two big categories based on how they are stored and copied in memory.

1. Primitive Data Types

here are 7 primitive types in JavaScript:

| Type | Example | Description |
|---|---|---|
| `string` | `"hello"` | Text |
| `number` | `42`, `3.14` | Integer or float (also `NaN`, `Infinity`) |
| `boolean` | `true`, `false` | Logical value |
| `undefined` | `undefined` | Declared but not assigned |
| `null` | `null` | Intentional "no value" |
| `symbol` | `Symbol("id")` | Unique, immutable identifier (ES6) |
| `bigint` | `123n` | Arbitrarily large integers (ES2020) |

Key characteristics

a. `Immutable` — You can't change the value itself; you can only reassign the variable.

```
let str = "hello";
str.toUpperCase();   // "HELLO" — returns a new string
console.log(str);    // "hello" — original unchanged

```

b. `Copied by value` — Assigning to another variable copies the value.

```

let a = 10;
let b = a;
b = 20;

console.log(a); // 10 (unchanged)
console.log(b); // 20

```

c. `Compared by value`

```
"abc" === "abc"  // true
5 === 5          // true

```

2. Non-Primitive (Reference) Data Types

- There is really one non-primitive type: `object` — but it has many forms:

| Type | Example |
|---|---|
| Plain object | `{ name: "Alice" }` |
| Array | `[1, 2, 3]` |
| Function | `function() {}` |
| Date | `new Date()` |
| RegExp | `/abc/` |
| Map / Set | `new Map()` |
| Class instances | `new Person()` |


Key characteristics

a. `Mutable` — You can change their contents.

```
const obj = { name: "Alice" };
obj.name = "Bob";        // ✅ allowed
console.log(obj.name);   // "Bob"

```

b. `Copied by reference` — Assigning copies the address, not the data. Both variables point to the same object.

```
let a = { count: 1 };
let b = a;        // b points to the SAME object

b.count = 99;

console.log(a.count); // 99 — a is affected too!

```

c. `Compared by reference`

- Two objects with identical contents are not equal

```
{ name: "A" } === { name: "A" }  // false (different objects)
[1,2] === [1,2]                  // false

let x = { a: 1 };
let y = x;
x === y;                         // true (same reference)

```

d. Shallow vs Deep Copy

- Shallow copy (top level only)

```
const original = { a: 1, nested: { b: 2 } };
const shallow = { ...original };      // or Object.assign({}, original)

shallow.nested.b = 99;
console.log(original.nested.b);       // 99 — nested object still shared!

```

- Deep copy

```
const deep = structuredClone(original);  // modern, built-in
// or: JSON.parse(JSON.stringify(original)) — older, has limitations

```

## 5. What is the difference between `function declarations` and `function expressions`?

- Both create functions, but they differ in syntax, hoisting, naming, and when they can be used.

1. Syntax

Function Declaration

- A statement that starts with the function keyword followed by a name:

```
function greet(name) {
  return "Hello, " + name;
}

```

Function Expression

- A function assigned to a variable or used as a value:

```

const greet = function (name) {
  return "Hello, " + name;
};

```
The function is part of an expression — it's being assigned, passed, or returned.


2. Hoisting — The Biggest Difference

- Function declarations are fully hoisted

You can call them before they appear in the code:

```
sayHi(); // ✅ Works

function sayHi() {
  console.log("Hi!");
}

```

```
sayHi(); // ❌ TypeError: sayHi is not a function

var sayHi = function () {
  console.log("Hi!");
};




```

```

sayHi(); // ❌ ReferenceError (TDZ)

const sayHi = function () {
  console.log("Hi!");
};

```

3. Naming

- Function declarations must have a name

```
function add(a, b) { return a + b; }  // ✅ required

```

- Function expressions can be anonymous or named

```
// Anonymous
const add = function (a, b) { return a + b; };

// Named function expression (NFE)
const add = function addFn(a, b) { return a + b; };

```

4. Arrow Functions (a Form of Function Expression)

- Arrow functions are always expressions — they have no declaration form:

```
const add = (a, b) => a + b;

```