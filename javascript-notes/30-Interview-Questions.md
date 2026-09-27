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

## 6.Explain closures in JavaScript with a practical example ?

A `closure` is a function that "remembers" the variables from the scope where it was created, even after that outer scope has finished executing.

```
function outer() {
  let count = 0;

  function inner() {
    count++;
    console.log(count);
  }

  return inner;
}

const counter = outer();
counter(); // 1
counter(); // 2
counter(); // 3

```

```
function createCounter(start = 0) {
  let count = start;

  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    value() {
      return count;
    },
  };
}

const counterA = createCounter();
const counterB = createCounter(100);

counterA.increment(); // 1
counterA.increment(); // 2

counterB.increment(); // 101

console.log(counterA.value()); // 2
console.log(counterB.value()); // 101

```

## 7 What is the difference between `call`, `apply`, and `bind`?

- All three methods let you control what this refers to inside a function
- The difference is how they pass arguments and whether they invoke the function immediately.

1. Basic Syntax

```
fn.call(thisArg, arg1, arg2, ...)
fn.apply(thisArg, [arg1, arg2, ...])
const bound = fn.bind(thisArg, arg1, arg2, ...)

```

`call` — Invoke with Arguments Listed

- Runs immediately.
- Arguments passed one by one.

example:

```
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: "Alice" };

greet.call(person, "Hello", "!");  // Hello, Alice!

```

`apply` — Invoke with Arguments in an Array
- Runs immediately.
- Arguments passed as an array (or array-like object).


example:

```
greet.apply(person, ["Hi", "?"]);  // Hi, Alice?

```

`bind ` — Create a New Function with Locked `this`
- Does not invoke the function.
- Returns a new function with `this` permanently bound.
- You can also preset arguments (partial application).

example:

```
const boundGreet = greet.bind(person, "Hey");
boundGreet("!!!"); // Hey, Alice!!!

```

```
function introduce(age, city) {
  console.log(`${this.name} is ${age} from ${city}`);
}

const user = { name: "Bob" };

// call — args listed
introduce.call(user, 30, "Delhi");
// Bob is 30 from Delhi

// apply — args in array
introduce.apply(user, [30, "Delhi"]);
// Bob is 30 from Delhi

// bind — returns a function
const boundIntro = introduce.bind(user, 30);
boundIntro("Mumbai");
// Bob is 30 from Mumbai

```

## 8 What are `arrow functions`, and how do they differ from `regular functions`?

- Arrow functions are a shorter syntax for writing functions, introduced in ES6 (2015).
- Beyond being concise, they behave differently from regular functions in several important ways

1. Syntax

```
// normal function
function add(a, b) {
  return a + b;
}


// arrow function
const add = (a, b) => a + b;

```

2. `this` Binding — The Biggest Difference

```
const counter = {
  count: 0,
  increment: function () {
    console.log(this.count++);
  },
};

counter.increment(); // 0, 1, 2...  (this = counter)

```

Arrow functions inherit this lexically (from where they're defined)

```
const counter = {
  count: 0,
  increment: () => {
    console.log(this.count++); // this = outer scope, NOT counter
  },
};

counter.increment(); // NaN


```
Because of this, arrow functions are a poor choice for object methods — but they shine inside callbacks:


```
const counter = {
  count: 0,
  start() {
    setInterval(() => {
      this.count++; // arrow inherits `this` from start()
      console.log(this.count);
    }, 1000);
  },
};

```

2. No arguments Object
- Regular functions have an `arguments object` — arrow functions don't

```
function sum() {
  return Array.from(arguments).reduce((a, b) => a + b, 0);
}
sum(1, 2, 3); // 6



const sum = () => {
  console.log(arguments); // ❌ ReferenceError
};


const sum = (...nums) => nums.reduce((a, b) => a + b, 0);
sum(1, 2, 3); // 6

```

3. Cannot Be Used as Constructors

```
const Person = (name) => {
  this.name = name;
};

new Person("Alice"); // ❌ TypeError: Person is not a constructor

```

They also don't have a prototype property:

```

function Regular() {}
console.log(Regular.prototype); // {}

const Arrow = () => {};
console.log(Arrow.prototype);   // undefined

```

4. No Hoisting (as Expressions)

```
greet(); // ❌ ReferenceError

const greet = () => console.log("hi");

```

```
greet(); // ✅ Works

function greet() {
  console.log("hi");
}

```

## 9 What is the difference between null and undefined?

Both represent "no value" 

1. `undefined` — The Default "Nothing

- `undefined` is what JavaScript assigns automatically when a value hasn't been given.

example:

```

let a;
console.log(a); // undefined — declared but not assigned

function greet(name) {}
greet(); // name = undefined — argument not passed

const obj = {};
console.log(obj.missing); // undefined — property doesn't exist

function noReturn() {}
console.log(noReturn()); // undefined — no return statement

const arr = [1, 2, 3];
console.log(arr[10]); // undefined — index out of bounds

```

2. `null` — The Intentional "Nothing"

- `null` is a value you assign to explicitly say "this is empty on purpose."

example:

```
let user = null; // deliberately no user yet

// Later...
user = { name: "Alice" };

```

```
typeof undefined; // "undefined"
typeof null;      // "object"  ⚠️ historical bug from 1995

```

