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

