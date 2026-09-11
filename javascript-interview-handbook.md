# 🚀 Ultimate JavaScript Interview Handbook & Notes

A comprehensive, production-ready, question-and-answer interview preparation guide covering fundamental to advanced JavaScript concepts with real-world explanations, code snippets, and common interview traps.

---

## 📑 Table of Contents
1. [Fundamentals](#1-fundamentals)
2. [Functions & Execution Context](#2-functions--execution-context)
3. [Arrays & Objects](#3-arrays--objects)
4. [Asynchronous JavaScript & Event Loop](#4-asynchronous-javascript--event-loop)
5. [Advanced JavaScript & Performance](#5-advanced-javascript--performance)

---

# 1. Fundamentals

---

### Q1: What is JavaScript?
**Answer:**
JavaScript is a **high-level, single-threaded, garbage-collected, interpreted (or JIT-compiled), multi-paradigm, dynamic language with a non-blocking event loop concurrency model**.

- **Single-threaded:** Executes one command at a time on a single call stack.
- **Dynamic:** Types are bound to values, not variables (weakly typed).
- **Multi-paradigm:** Supports Object-Oriented (prototypal), Functional, and Imperative programming styles.

---

### Q2: JavaScript vs Java?
**Answer:**

| Feature | JavaScript | Java |
| :--- | :--- | :--- |
| **Type System** | Dynamic / Weakly Typed | Static / Strongly Typed |
| **Execution** | Interpreted / JIT-compiled (V8, SpiderMonkey) in browser/Node.js | Compiled to Bytecode (.class) and executed on JVM |
| **Object Model**| Prototypal inheritance | Classical Class-based inheritance |
| **Concurrency** | Single-threaded with non-blocking Event Loop | Multi-threaded with thread pools & shared memory |
| **Use Case** | Web dev (Frontend + Backend), mobile, tooling | Enterprise backends, Android apps, large distributed systems |

---

### Q3: `var` vs `let` vs `const`
**Answer:**

| Feature | `var` | `let` | `const` |
| :--- | :--- | :--- | :--- |
| **Scope** | Function scope | Block scope `{}` | Block scope `{}` |
| **Hoisting** | Hoisted and initialized with `undefined` | Hoisted but in **Temporal Dead Zone (TDZ)** | Hoisted but in **Temporal Dead Zone (TDZ)** |
| **Re-declaration**| Allowed in same scope | Not allowed | Not allowed |
| **Re-assignment** | Allowed | Allowed | Not allowed |
| **Global Object Attachment** | Attaches to `window`/`globalThis` | Does not attach | Does not attach |

```javascript
// var example
var a = 10;
var a = 20; // Allowed

// let example
let b = 10;
// let b = 20; // SyntaxError: Identifier 'b' has already been declared
b = 30; // Allowed

// const example
const c = 10;
// c = 20; // TypeError: Assignment to constant variable.

// const with objects (mutating properties is allowed, reassignment is not)
const user = { name: "Sangam" };
user.name = "John"; // Allowed!
// user = {}; // TypeError!
```

---

### Q4: What is scope?
**Answer:**
**Scope** is the current context of execution determining the accessibility (visibility) of variables, functions, and objects in your code. It prevents variable collisions and provides security/encapsulation.

---

### Q5: Global scope vs function scope vs block scope
**Answer:**
1. **Global Scope:** Variables declared outside any function or block. Accessible anywhere in the program.
2. **Function (Local) Scope:** Variables declared inside a function (using `var`, `let`, or `const`). Accessible only inside that function.
3. **Block Scope:** Variables declared with `let` or `const` inside a pair of curly braces `{}`.

```javascript
// Global Scope
let globalVar = "I am global";

function testFunctionScope() {
  var funcVar = "I am function scoped";
  if (true) {
    let blockVar = "I am block scoped";
    var hoistedInside = "I leak to function scope";
    console.log(blockVar); // Accessible
  }
  // console.log(blockVar); // ReferenceError: blockVar is not defined
  console.log(hoistedInside); // Accessible
}
// console.log(funcVar); // ReferenceError: funcVar is not defined
```

---

### Q6: What is hoisting?
**Answer:**
**Hoisting** is JavaScript's default behavior of moving variable and function declarations to the top of their containing scope during the **Memory Creation Phase** before code execution.

---

### Q7: How does hoisting work with `var`?
**Answer:**
Variables declared with `var` are hoisted to the top of their function or global scope and initialized with `undefined`.

```javascript
console.log(x); // Output: undefined (no error!)
var x = 5;
console.log(x); // Output: 5
```

---

### Q8: How does hoisting work with `let` and `const`?
**Answer:**
Variables declared with `let` and `const` **are also hoisted**, but they are **not initialized**. They stay uninitialized in the **Temporal Dead Zone (TDZ)** from the start of the block until the execution reaches the declaration line.

```javascript
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
```

---

### Q9: What is the Temporal Dead Zone (TDZ)?
**Answer:**
The **Temporal Dead Zone (TDZ)** is the time period/region of code execution between entering the scope and the actual line where a `let` or `const` variable is declared and initialized. Accessing the variable in this zone throws a `ReferenceError`.

```javascript
{
  // TDZ for myVar starts here
  // console.log(myVar); // ReferenceError
  let x = 1; 
  let myVar = 10; // TDZ ends here for myVar
  console.log(myVar); // 10
}
```

---

### Q10: What are primitive data types?
**Answer:**
Primitive types hold a single, immutable value directly on the **Stack memory**.
There are **7 primitive types** in JS:
1. `string`
2. `number`
3. `boolean`
4. `null`
5. `undefined`
6. `symbol` (ES6)
7. `bigint` (ES2020)

---

### Q11: What are reference data types?
**Answer:**
Reference types store objects where the variable holds a memory address (reference) pointing to the actual data stored on the **Heap memory**.
- `Object` (including plain objects `{}`)
- `Array` `[]`
- `Function`
- `Date`, `RegExp`, `Map`, `Set`, etc.

---

### Q12: Primitive vs reference types
**Answer:**

| Category | Primitive Types | Reference Types |
| :--- | :--- | :--- |
| **Storage** | Stored directly in Stack memory | Reference in Stack, actual data in Heap |
| **Mutability** | Immutable (value cannot be mutated directly) | Mutable (properties can be changed) |
| **Comparison** | Compared by **value** | Compared by **memory reference** |

```javascript
// Primitive comparison
let str1 = "hello";
let str2 = "hello";
console.log(str1 === str2); // true

// Reference comparison
let obj1 = { val: 10 };
let obj2 = { val: 10 };
console.log(obj1 === obj2); // false (different memory references)
```

---

### Q13: What is pass by value?
**Answer:**
In pass by value, a copy of the actual value is passed to a function. Changes made inside the function do not affect the original variable. **All primitive data types are passed by value.**

```javascript
function modify(val) {
  val = 100;
}
let num = 10;
modify(num);
console.log(num); // 10 (unchanged)
```

---

### Q14: What is pass by reference?
**Answer:**
When passing objects/arrays into functions, a copy of the **reference (memory pointer)** is passed (often called *pass by sharing*). Mutating properties through this reference mutates the underlying object. However, reassigning the variable does not affect the outer reference.

```javascript
function updateObject(obj) {
  obj.name = "Changed"; // Mutates the original object
}
let person = { name: "Original" };
updateObject(person);
console.log(person.name); // "Changed"

function reassignObject(obj) {
  obj = { name: "New" }; // Reassignment breaks link
}
reassignObject(person);
console.log(person.name); // Still "Changed"
```

---

### Q15: `==` vs `===`
**Answer:**
- `==` (**Loose Equality / Abstract Equality**): Compares values **with type coercion**. Converts both operands to a common type before comparison.
- `===` (**Strict Equality**): Compares both **value AND type** without any coercion.

```javascript
0 == false;    // true  (Number(false) -> 0)
"" == false;   // true  (Number("") -> 0, Number(false) -> 0)
0 === false;   // false (number !== boolean)
null == undefined;  // true  (special JS rule)
null === undefined; // false
```

---

### Q16: `null` vs `undefined`
**Answer:**
- `undefined`: The variable has been declared but has **not yet been assigned a value**. It is the default return value of functions without a return statement, uninitialized variables, or missing object keys.
- `null`: An intentional assignment value representing **empty or no value**.

```javascript
let a;
console.log(a); // undefined
console.log(typeof a); // "undefined"

let b = null;
console.log(b); // null
console.log(typeof b); // "object" (famous historical JS bug)

console.log(null == undefined);  // true
console.log(null === undefined); // false
```

---

### Q17: What is `NaN`?
**Answer:**
`NaN` stands for **"Not-a-Number"**. It is a special numeric value returned when a mathematical operation fails or cannot produce a meaningful numeric result.
> *Note:* `typeof NaN` is `"number"`.

```javascript
console.log("hello" / 2); // NaN
console.log(Math.sqrt(-1)); // NaN
console.log(typeof NaN); // "number"
console.log(NaN === NaN); // false (NaN is the only value in JS not equal to itself!)
```

---

### Q18: How do you check if something is `NaN`?
**Answer:**
1. `Number.isNaN(value)` (Recommended): Checks if the value is strictly `NaN` without type coercion.
2. `isNaN(value)`: Coerces value to a number first, then checks if it is `NaN` (can produce false positives).
3. `Object.is(value, NaN)` or checking if `value !== value`.

```javascript
isNaN("hello");        // true (coerces "hello" to NaN)
Number.isNaN("hello"); // false ("hello" is a string, not NaN)
Number.isNaN(NaN);     // true

function isStrictNaN(val) {
  return val !== val;
}
```

---

### Q19: What is type coercion?
**Answer:**
**Type coercion** is the automatic or implicit conversion of values from one data type to another by JavaScript runtime (e.g., string to number, number to boolean) during operations like addition, comparison, or logical conditions.

---

### Q20: Implicit vs explicit type conversion
**Answer:**
- **Implicit (Coercion):** Handled automatically by JS engine.
- **Explicit (Type Casting):** Triggered intentionally by developer using constructors or methods (`Number()`, `String()`, `Boolean()`, `parseInt()`).

```javascript
// Implicit Conversion
console.log("5" + 2); // "52" (number 2 coerced to string)
console.log("5" - 2); // 3 (string "5" coerced to number)
console.log(true + 1); // 2 (true -> 1)

// Explicit Conversion
console.log(Number("5") + 2); // 7
console.log(String(123));     // "123"
console.log(Boolean(0));      // false
```

---

# 2. Functions & Execution Context

---

### Q21: Function declaration vs function expression
**Answer:**
- **Function Declaration:** Declared as a standalone statement. **Hoisted completely** with its body, so it can be called before its definition.
- **Function Expression:** A function assigned to a variable. Only the variable is hoisted (subject to `var`/`let` rules), not the function body.

```javascript
// Function Declaration
sayHello(); // Works! Output: "Hello"
function sayHello() {
  console.log("Hello");
}

// Function Expression
// sayHi(); // TypeError: sayHi is not a function (if var) or ReferenceError (if let/const)
const sayHi = function() {
  console.log("Hi");
};
```

---

### Q22: What are arrow functions?
**Answer:**
Introduced in ES6, **Arrow Functions** provide a concise syntax for writing function expressions. They have lexical scoping for `this`, `arguments`, and `super`, and cannot be used as constructors.

```javascript
// Concise arrow function
const add = (a, b) => a + b;
```

---

### Q23: Arrow function vs normal function
**Answer:**

| Feature | Regular Function | Arrow Function |
| :--- | :--- | :--- |
| **`this` Binding** | Dynamic (determined by *how* it is called) | Lexical (inherits `this` from enclosing scope) |
| **`arguments` Object**| Available | Not available (use Rest params `...args`) |
| **Constructible (`new`)**| Yes, can be called with `new` | No (`TypeError: ... is not a constructor`) |
| **Duplicate Parameters**| Allowed in non-strict mode | Not allowed |
| **Hoisting** | Function declarations are fully hoisted | Subject to variable declaration hoisting rules |

---

### Q24: What is a callback function?
**Answer:**
A **callback function** is a function passed as an argument to another function, which is then invoked inside that outer function to complete a routine or action (synchronous or asynchronous).

```javascript
// Synchronous callback
[1, 2, 3].map((item) => item * 2);

// Asynchronous callback
setTimeout(() => {
  console.log("Executed after 1s");
}, 1000);
```

---

### Q25: What is a higher-order function (HOF)?
**Answer:**
A **Higher-Order Function** is a function that either:
1. Takes one or more functions as arguments (e.g., `map`, `filter`, `reduce`), OR
2. Returns a function as its result.

```javascript
function multiplyBy(factor) {
  return function(number) {
    return number * factor;
  };
}
const double = multiplyBy(2);
console.log(double(5)); // 10
```

---

### Q26: What is a pure function?
**Answer:**
A **Pure Function** is a function that:
1. **Deterministic:** Given the same inputs, it always returns the exact same output.
2. **No Side Effects:** It does not modify any external state, global variables, DOM, or mutate its arguments.

```javascript
// Pure
const add = (a, b) => a + b;

// Impure (Modifies external state)
let total = 0;
const addToTotal = (amount) => {
  total += amount; // Side effect!
  return total;
};
```

---

### Q27: What is an IIFE (Immediately Invoked Function Expression)?
**Answer:**
An **IIFE** is a JavaScript function that runs as soon as it is defined. It creates a private lexical scope to prevent polluting the global namespace.

```javascript
(function () {
  const secret = "private_key";
  console.log("IIFE executed immediately");
})();
// console.log(secret); // ReferenceError
```

---

### Q28: What is recursion?
**Answer:**
**Recursion** is a programming technique where a function calls itself until it hits a defined **base condition (stopping criterion)**. Without a base condition, it triggers a **Maximum call stack size exceeded (Stack Overflow)** error.

```javascript
function factorial(n) {
  if (n <= 1) return 1; // Base case
  return n * factorial(n - 1); // Recursive call
}
console.log(factorial(5)); // 120
```

---

### Q29: What is a closure?
**Answer:**
A **closure** is the combination of a function bundled together with references to its surrounding state (**lexical environment**). A closure gives an inner function access to an outer function's scope even **after the outer function has finished executing**.

```javascript
function createCounter() {
  let count = 0; // Private variable
  return {
    increment: () => ++count,
    getCount: () => count,
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
```

---

### Q30: Real-world use case of closure
**Answer:**
1. **Data Privacy / Encapsulation:** Creating private variables and methods (Module pattern).
2. **Function Currying & Partial Application:** Pre-filling arguments.
3. **Event Listeners / State retention in UI:** Retaining state or configuration.
4. **Memoization / Caching:** Storing previous computation results.
5. **Debouncing and Throttling functions**.

```javascript
// Data privacy / factory pattern
function createWallet(initialBalance) {
  let balance = initialBalance; // private
  return {
    deposit: (amount) => { balance += amount; },
    getBalance: () => balance
  };
}
```

---

### Q31: What is lexical scope?
**Answer:**
**Lexical Scope (Static Scope)** means the accessibility of variables is determined by their physical location (where they are written) in the source code. Inner functions have access to variables declared in their outer parent scopes.

---

### Q32: What is the scope chain?
**Answer:**
When a variable is accessed, the JavaScript engine tries to find it in the current local scope. If not found, it moves up to the parent scope, continuing step-by-step up to the Global Scope. This hierarchical chain of scopes is called the **Scope Chain**. If not found anywhere, it throws a `ReferenceError`.

---

### Q33: What is `this`?
**Answer:**
`this` refers to the object representing the context in which the current code is being executed.
- **In an object method:** `this` refers to the object calling the method.
- **In a regular function (non-strict):** `this` refers to the global object (`window` or `global`).
- **In strict mode (`"use strict"`):** `this` inside a plain function is `undefined`.
- **In DOM event handler:** `this` points to the DOM element that received the event.
- **Inside constructor / `class`:** `this` refers to the newly created instance.

---

### Q34: How does `this` behave inside an arrow function?
**Answer:**
Arrow functions **do not have their own `this` binding**. Instead, they capture the `this` value of the enclosing lexical context at the time they are created. Calling `call()`, `apply()`, or `bind()` on an arrow function has **no effect** on `this`.

```javascript
const obj = {
  name: "Sangam",
  regularFn: function() {
    console.log("regular:", this.name);
  },
  arrowFn: () => {
    console.log("arrow:", this.name);
  },
  delayedFn: function() {
    setTimeout(() => {
      console.log("delayed arrow:", this.name); // Correctly refers to obj
    }, 100);
  }
};

obj.regularFn(); // "Sangam"
obj.arrowFn();   // undefined (points to window/global)
obj.delayedFn(); // "Sangam"
```

---

### Q35: `call()` vs `apply()` vs `bind()`
**Answer:**
All three methods are used to set the explicit `this` context for a function.

| Method | Execution | Arguments Passing | Returns |
| :--- | :--- | :--- | :--- |
| `call()` | Invokes immediately | Comma-separated list (`arg1, arg2`) | Result of function |
| `apply()`| Invokes immediately | Single Array (`[arg1, arg2]`) | Result of function |
| `bind()` | Does **not** invoke immediately | Comma-separated list (supports partial application) | New function with locked `this` |

```javascript
const person = { name: "Sangam" };

function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

// call
console.log(greet.call(person, "Hello", "!")); // "Hello, Sangam!"

// apply
console.log(greet.apply(person, ["Hi", "?"])); // "Hi, Sangam?"

// bind
const boundGreet = greet.bind(person, "Hey");
console.log(boundGreet("!")); // "Hey, Sangam!"
```

---

### Q36: What is currying?
**Answer:**
**Currying** is a functional programming technique where a function with multiple arguments `f(a, b, c)` is transformed into a sequence of nested functions each taking a single argument `f(a)(b)(c)`.

```javascript
// Normal
const add = (a, b, c) => a + b + c;

// Curried
const curriedAdd = (a) => (b) => (c) => a + b + c;
console.log(curriedAdd(1)(2)(3)); // 6
```

---

### Q37: What is function composition?
**Answer:**
**Function Composition** is the process of combining two or more functions to produce a new function. The output of one function becomes the input of the next: `(f ∘ g)(x) = f(g(x))`.

```javascript
const compose = (f, g) => (x) => f(g(x));

const toUpperCase = (str) => str.toUpperCase();
const exclaim = (str) => `${str}!`;

const shout = compose(exclaim, toUpperCase);
console.log(shout("hello")); // "HELLO!"
```

---

# 3. Arrays / Objects

---

### Q38: `map()` vs `forEach()`
**Answer:**

| Feature | `map()` | `forEach()` |
| :--- | :--- | :--- |
| **Return Value** | Returns a **new array** with transformed values | Returns `undefined` |
| **Chainability** | Can chain other array methods (`.filter()`, etc.) | Cannot be chained |
| **Original Array**| Does not mutate original (unless mutated manually)| Does not mutate directly, used for side-effects |
| **Performance** | Optimized for transformation | Used for iteration/side-effects (logging, saving) |

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map(x => x * 2); // [2, 4, 6]
const res = numbers.forEach(x => console.log(x)); // undefined
```

---

### Q39: `filter()` vs `find()`
**Answer:**
- `filter()`: Iterates through the entire array and returns an **array of all matching elements**. If no match, returns `[]`.
- `find()`: Returns the **first matching element value** and stops iteration immediately. If no match, returns `undefined`.

```javascript
const users = [{ id: 1, role: "admin" }, { id: 2, role: "user" }, { id: 3, role: "admin" }];

users.filter(u => u.role === "admin"); // [{id:1, role:"admin"}, {id:3, role:"admin"}]
users.find(u => u.role === "admin");   // {id:1, role:"admin"}
```

---

### Q40: `map()` vs `reduce()`
**Answer:**
- `map()`: Transforms an array into a new array of the exact same length (1-to-1 transformation).
- `reduce()`: Accumulates array elements into a **single output value** (number, object, array, string).

```javascript
const nums = [1, 2, 3, 4];
// map
const squared = nums.map(n => n * n); // [1, 4, 9, 16]

// reduce
const sum = nums.reduce((acc, curr) => acc + curr, 0); // 10
```

---

### Q41: `some()` vs `every()`
**Answer:**
- `some()`: Returns `true` if **at least one element** matches the predicate condition (stops early).
- `every()`: Returns `true` if **all elements** match the predicate condition (stops early on first `false`).

```javascript
const scores = [85, 92, 45, 78];
console.log(scores.some(s => s < 50));  // true (45 < 50)
console.log(scores.every(s => s >= 50)); // false
```

---

### Q42: `find()` vs `findIndex()`
**Answer:**
- `find()`: Returns the **value** of the first element satisfying the condition (`undefined` if not found).
- `findIndex()`: Returns the **index (0-based)** of the first element satisfying the condition (`-1` if not found).

---

### Q43: `slice()` vs `splice()`
**Answer:**

| Feature | `slice(start, end)` | `splice(start, deleteCount, ...items)` |
| :--- | :--- | :--- |
| **Mutation** | **Pure**: Does NOT mutate original array | **Impure**: Mutates original array in place |
| **Return Value** | Returns a shallow copy of extracted section | Returns array of deleted items |
| **Purpose** | Read / extract portion of array | Add, remove, or replace elements |

```javascript
const arr = ["a", "b", "c", "d"];

const sliced = arr.slice(1, 3);
console.log(sliced); // ["b", "c"]
console.log(arr);    // ["a", "b", "c", "d"] (unchanged)

const spliced = arr.splice(1, 2, "x", "y");
console.log(spliced); // ["b", "c"]
console.log(arr);     // ["a", "x", "y", "d"] (mutated!)
```

---

### Q44: How do you remove duplicates from an array?
**Answer:**

```javascript
const arr = [1, 2, 2, 3, 4, 4, 5];

// Method 1: Using Set (Most common & cleanest)
const unique1 = [...new Set(arr)]; // [1, 2, 3, 4, 5]

// Method 2: Using filter & indexOf
const unique2 = arr.filter((item, index) => arr.indexOf(item) === index);

// Method 3: Using reduce
const unique3 = arr.reduce((acc, curr) => {
  if (!acc.includes(curr)) acc.push(curr);
  return acc;
}, []);
```

---

### Q45: How do you find duplicate elements?
**Answer:**

```javascript
const arr = [1, 2, 3, 2, 4, 5, 3, 6];

const duplicates = arr.filter((item, index) => arr.indexOf(item) !== index);
console.log([...new Set(duplicates)]); // [2, 3]
```

---

### Q46: How do you count frequency of elements?
**Answer:**

```javascript
const fruits = ["apple", "banana", "apple", "orange", "banana", "apple"];

// Using reduce
const frequency = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});

console.log(frequency);
// { apple: 3, banana: 2, orange: 1 }
```

---

### Q47: How do you flatten an array?
**Answer:**

```javascript
const nested = [1, [2, [3, [4, 5]]]];

// 1. Array.prototype.flat() (ES2019)
console.log(nested.flat(Infinity)); // [1, 2, 3, 4, 5]

// 2. Custom recursive function (Common Interview Question)
function customFlatten(arr) {
  let result = [];
  for (let item of arr) {
    if (Array.isArray(item)) {
      result.push(...customFlatten(item));
    } else {
      result.push(item);
    }
  }
  return result;
}
console.log(customFlatten(nested)); // [1, 2, 3, 4, 5]
```

---

### Q48: How do you sort an array?
**Answer:**
Array `.sort()` sorts elements in-place. By default, it converts elements to **strings** and compares their UTF-16 code unit values. For numbers or custom objects, you **must provide a compare function**.

```javascript
// Numbers sorting
const numbers = [40, 100, 1, 5, 25, 10];
numbers.sort((a, b) => a - b); // Ascending: [1, 5, 10, 25, 40, 100]
numbers.sort((a, b) => b - a); // Descending: [100, 40, 25, 10, 5, 1]

// Object sorting by property
const users = [{ age: 25 }, { age: 19 }, { age: 30 }];
users.sort((a, b) => a.age - b.age);
```

---

### Q49: Why does `[10, 2, 5].sort()` give unexpected results?
**Answer:**
Because `Array.prototype.sort()` converts elements to strings before sorting alphabetically:
`"10"` comes before `"2"` in lexicographical order (ASCII), resulting in `[10, 2, 5]`.
To fix it, provide a numeric comparator: `arr.sort((a, b) => a - b)`.

---

### Q50: Object destructuring
**Answer:**
Extracting properties from objects into distinct variables with concise syntax, renaming, and default values.

```javascript
const user = { id: 101, name: "Sangam", role: "admin" };

// Basic destructuring with alias and default value
const { name: userName, role, age = 25 } = user;
console.log(userName, role, age); // "Sangam", "admin", 25
```

---

### Q51: Array destructuring
**Answer:**
Extracting items from arrays based on position.

```javascript
const coords = [10, 20, 30, 40];
const [x, y, ...rest] = coords;
console.log(x, y); // 10, 20
console.log(rest); // [30, 40]

// Value swapping trick
let a = 1, b = 2;
[a, b] = [b, a]; // a = 2, b = 1
```

---

### Q52: Spread operator (`...`)
**Answer:**
Unpacks/expands iterable elements (arrays, objects, strings) into individual elements.

```javascript
// Array spread
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4]; // [1, 2, 3, 4]

// Object spread
const base = { theme: "dark" };
const config = { ...base, showSidebar: true };
```

---

### Q53: Rest operator (`...`)
**Answer:**
Gathers multiple elements into a single array or object parameter. It must be the **last** parameter.

```javascript
function sum(...numbers) {
  return numbers.reduce((acc, curr) => acc + curr, 0);
}
console.log(sum(1, 2, 3, 4)); // 10
```

---

### Q54: Shallow copy vs deep copy
**Answer:**
- **Shallow Copy:** Copies top-level properties. Nested objects or arrays are copied by reference (changes to nested objects affect both copies).
- **Deep Copy:** Recursively copies all levels of an object/array, creating a completely independent copy in memory.

```javascript
const original = { a: 1, nested: { b: 2 } };

// Shallow Copy (Spread / Object.assign)
const shallow = { ...original };
shallow.nested.b = 999;
console.log(original.nested.b); // 999 (Mutated!)

// Deep Copy (structuredClone in modern JS)
const deep = structuredClone(original);
deep.nested.b = 500;
console.log(original.nested.b); // 999 (Unaffected)
```

---

### Q55: `Object.assign()` vs Spread (`...`)
**Answer:**
- Both perform shallow copy.
- `Object.assign()` mutates the target object passed as the first argument (`Object.assign(target, ...sources)`).
- Spread syntax creates a new object literal expression and is generally cleaner and faster.
- `Object.assign()` invokes setters on the target object, whereas spread directly defines new properties.

---

### Q56: `Object.freeze()`
**Answer:**
Freezes an object completely:
- Cannot add new properties.
- Cannot delete existing properties.
- Cannot modify values of existing properties.
- Cannot modify property descriptors (non-configurable).
*(Note: Shallow freeze only; nested objects are still mutable unless recursively frozen).*

```javascript
const config = Object.freeze({ api: "https://api.com" });
config.api = "new-url"; // Fails silently (or TypeError in strict mode)
console.log(config.api); // "https://api.com"
```

---

### Q57: `Object.seal()`
**Answer:**
Seals an object:
- Cannot add new properties.
- Cannot delete existing properties.
- **Can modify existing property values** (as long as they are writable).

---

### Q58: What are computed properties?
**Answer:**
Computed property names allow dynamic object key evaluation using square brackets `[]` inside object literals.

```javascript
const key = "user_role";
const person = {
  name: "Sangam",
  [key]: "admin", // Dynamic property key
};
console.log(person.user_role); // "admin"
```

---

### Q59: What is optional chaining (`?.`)?
**Answer:**
The optional chaining operator (`?.`) allows safe reading of properties nested deep within an object without having to explicitly validate each reference in the chain. If a reference is `null` or `undefined`, the expression short-circuits and evaluates to `undefined` without throwing a `TypeError`.

```javascript
const user = { profile: null };
// Without optional chaining: user.profile.avatar -> TypeError!
console.log(user?.profile?.avatar); // undefined
console.log(user.getDetails?.());   // undefined
```

---

### Q60: What is nullish coalescing (`??`)?
**Answer:**
The **nullish coalescing operator (`??`)** is a logical operator that returns its right-hand side operand when its left-hand side operand is **`null` or `undefined`**, and otherwise returns its left-hand side operand.
*(Unlike `||`, it does NOT treat `0`, `""`, or `false` as falsy).*

```javascript
const count = 0;

console.log(count || 10); // 10 (Wrong! 0 is considered falsy by ||)
console.log(count ?? 10); // 0 (Correct! 0 is not null or undefined)
```

---

# 4. Asynchronous JavaScript & Event Loop

---

### Q61: Synchronous vs Asynchronous JavaScript
**Answer:**
- **Synchronous:** Code is executed line-by-line sequentially on the main thread. A slow or blocking operation pauses subsequent code execution.
- **Asynchronous:** Long-running operations (Network requests, Timers, File I/O) are offloaded to runtime Web APIs. The main thread remains unblocked. When the async operation completes, its callback is scheduled to run on the call stack via the Event Loop.

---

### Q62: What is the Event Loop?
**Answer:**
The **Event Loop** is a continuously running background mechanism that monitors the **Call Stack** and the **Callback / Microtask Queues**.
If the Call Stack is completely **empty**, the Event Loop takes queued callback functions and pushes them onto the Call Stack for execution.

---

### Q63: Call Stack
**Answer:**
A LIFO (Last In, First Out) data structure that tracks function execution contexts in JavaScript. When a function is invoked, its frame is pushed to the stack; when it returns, it is popped off.

---

### Q64: Web APIs
**Answer:**
Browser-provided (or Node.js runtime) threads/APIs that handle async operations outside the JS single thread:
- `DOM API`
- `fetch()` / `XMLHttpRequest`
- `setTimeout()` / `setInterval()`
- Geolocation, IndexedDB, Storage, etc.

---

### Q65: Callback Queue (Task / Macrotask Queue)
**Answer:**
A FIFO (First In, First Out) queue holding callbacks from macrotasks like `setTimeout`, `setInterval`, `setImmediate` (Node), DOM events, and I/O callbacks, waiting to be processed by the event loop.

---

### Q66: Microtask Queue
**Answer:**
A high-priority FIFO queue holding callbacks from:
- `Promise.then()`, `.catch()`, `.finally()`
- `queueMicrotask()`
- `MutationObserver`
- `process.nextTick()` (Node.js specific, runs before standard microtasks)

> **Critical Rule:** The Event Loop **always drains the entire Microtask Queue** before picking the next task from the Macrotask Queue.

---

### Q67: Macrotask Queue
**Answer:**
Same as the Callback Queue. Includes tasks like `setTimeout`, `setInterval`, `setImmediate`, UI rendering, and user input events.

---

### Q68: Promise
**Answer:**
A **Promise** is an object representing the eventual completion (or failure) of an asynchronous operation and its resulting value.

```javascript
const fetchUserData = new Promise((resolve, reject) => {
  let success = true;
  if (success) {
    resolve({ id: 1, name: "Sangam" });
  } else {
    reject(new Error("Network Error"));
  }
});
```

---

### Q69: Promise states
**Answer:**
A Promise is always in one of 3 mutually exclusive states:
1. **Pending:** Initial state, neither fulfilled nor rejected.
2. **Fulfilled:** The operation completed successfully (`resolve()` was called).
3. **Rejected:** The operation failed (`reject()` was called).

---

### Q70: Promise chaining
**Answer:**
Executing asynchronous tasks sequentially by chaining `.then()` handlers. Each `.then()` returns a new Promise, allowing values or promises to be passed down the chain.

```javascript
fetchUser()
  .then(user => fetchOrders(user.id))
  .then(orders => calculateTotal(orders))
  .then(total => console.log("Total:", total))
  .catch(err => console.error("Error anywhere in chain:", err));
```

---

### Q71: `async` / `await`
**Answer:**
Syntactic sugar built on top of Promises introduced in ES2017:
- `async` keyword placed before a function ensures it always returns a Promise.
- `await` pauses function execution until the Promise resolves or rejects, making async code read like synchronous code without blocking the main thread.

```javascript
async function getUser() {
  try {
    const response = await fetch("/api/user");
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Failed to fetch:", error);
  }
}
```

---

### Q72: `Promise.all()`
**Answer:**
Takes an iterable of Promises and executes them concurrently.
- **Resolves:** When **all** promises resolve (returns an array of results in order).
- **Rejects (Fail-Fast):** As soon as **any one promise rejects**, immediately rejecting with that error.

```javascript
Promise.all([p1, p2, p3])
  .then(([r1, r2, r3]) => console.log(r1, r2, r3))
  .catch(err => console.error("First failure:", err));
```

---

### Q73: `Promise.allSettled()`
**Answer:**
Waits for **all** promises to settle (either fulfilled or rejected). Never short-circuits on failure.
Returns an array of objects describing the outcome of each promise:
`{ status: "fulfilled", value: ... }` or `{ status: "rejected", reason: ... }`.

```javascript
Promise.allSettled([p1, p2]).then(results => {
  results.forEach(res => {
    if (res.status === "fulfilled") console.log("Value:", res.value);
    if (res.status === "rejected") console.error("Reason:", res.reason);
  });
});
```

---

### Q74: `Promise.race()`
**Answer:**
Returns a promise that fulfills or rejects as soon as the **very first promise** in the iterable settles (resolves OR rejects).

---

### Q75: `Promise.any()`
**Answer:**
Returns a promise that resolves as soon as the **first promise fulfills** (ignores rejections until all fail). If all fail, it rejects with an `AggregateError`.

---

### Q76: `Promise.all()` vs `Promise.allSettled()`
**Answer:**

| Feature | `Promise.all()` | `Promise.allSettled()` |
| :--- | :--- | :--- |
| **Short-circuiting** | Yes, aborts on first rejection | No, waits for all promises to finish |
| **Return format** | Array of values `[val1, val2]` | Array of status objects `[{status, value}, {status, reason}]` |
| **Use Case** | Dependent parallel tasks where all must succeed | Independent tasks where partial failure is acceptable |

---

### Q77: What happens when one Promise fails?
**Answer:**
- In `Promise.all()`: The entire call rejects immediately with the error.
- In `Promise.allSettled()`: Captured in the output array with `{ status: "rejected", reason: err }`.
- In `Promise.race()`: Rejects if the failed promise settled first.
- In unhandled promises: Emits an `UnhandledPromiseRejection` warning/error.

---

### Q78: How does `try/catch` work with `async/await`?
**Answer:**
`try/catch` blocks synchronously catch rejected promises awaited within an `async` function.

```javascript
async function run() {
  try {
    const data = await Promise.reject(new Error("Boom!"));
  } catch (error) {
    console.log("Caught:", error.message); // "Caught: Boom!"
  } finally {
    console.log("Cleanup actions");
  }
}
```

---

### Q79: Callback Hell
**Answer:**
Also known as the **"Pyramid of Doom"**, Callback Hell occurs when multiple asynchronous operations are nested within callbacks. It leads to unreadable, unmaintainable, and error-prone code.

```javascript
// Callback Hell
getUser(userId, (user) => {
  getOrders(user.id, (orders) => {
    getOrderDetails(orders[0].id, (details) => {
      // Deep nesting...
    });
  });
});
```

---

### Q80: How do you avoid callback hell?
**Answer:**
1. Using **Promises with chaining**.
2. Using **`async` / `await`** (cleanest approach).
3. Modularizing functions into separate named functions.

---

### Q81: What is the Event Loop execution order?
**Answer:**
1. Execute synchronous code on **Call Stack**.
2. When Call Stack is empty, process all **Microtasks** (Promises, `queueMicrotask`) until the Microtask Queue is completely drained.
3. Render UI / Repaint (in browser environment if necessary).
4. Pick **one Macrotask** from Callback Queue (`setTimeout`, `setInterval`, I/O).
5. Repeat the cycle.

---

### Q82: Predict output involving `setTimeout`, `Promise`, and `console.log`
**Answer:**

```javascript
console.log("1");

setTimeout(() => {
  console.log("2");
}, 0);

Promise.resolve().then(() => {
  console.log("3");
}).then(() => {
  console.log("4");
});

console.log("5");
```

**Output:**
```
1
5
3
4
2
```

**Step-by-Step Explanation:**
1. `console.log("1")` runs synchronously -> prints `1`.
2. `setTimeout` is sent to Web APIs; its timer finishes and its callback is placed in the **Macrotask Queue**.
3. `Promise.resolve().then(...)` places callback for `"3"` into the **Microtask Queue**.
4. `console.log("5")` runs synchronously -> prints `5`.
5. Call stack is now empty. The Event Loop drains the **Microtask Queue**: prints `3`, queues `"4"`, prints `4`.
6. Event loop moves to the **Macrotask Queue**: prints `2`.

---

### Q83: Microtask vs Macrotask
**Answer:**

| Feature | Microtask | Macrotask |
| :--- | :--- | :--- |
| **Examples** | `Promise.then`, `queueMicrotask`, `MutationObserver` | `setTimeout`, `setInterval`, `setImmediate`, DOM events |
| **Priority** | High priority | Lower priority |
| **Execution frequency** | Entire queue drained after every call stack execution | Executed one at a time per event loop turn |

---

### Q84: What happens internally when an API call is made?
**Answer:**
1. `fetch()` is executed on the Call Stack.
2. JS engine offloads network request to the **Browser/Host Web API network thread**.
3. `fetch()` returns a pending Promise to the Call Stack, and synchronous code continues executing.
4. When HTTP response returns from server, Web API resolves/rejects the Promise and places `.then()` callback onto the **Microtask Queue**.
5. Once the Call Stack is empty, Event Loop pushes the Microtask to Call Stack for execution.

---

### Q85: How would you handle multiple API calls simultaneously?
**Answer:**
Use `Promise.all()` or `Promise.allSettled()` to trigger requests concurrently rather than sequentially.

```javascript
async function loadDashboardData() {
  // Initiates all network requests simultaneously
  const [usersRes, productsRes, statsRes] = await Promise.all([
    fetch("/api/users"),
    fetch("/api/products"),
    fetch("/api/stats")
  ]);

  const users = await usersRes.json();
  const products = await productsRes.json();
  const stats = await statsRes.json();

  return { users, products, stats };
}
```

---

# 5. Advanced JS

---

### Q86: What is debouncing?
**Answer:**
**Debouncing** is an optimization pattern that guarantees a function will not be executed until a specified time delay has passed since the **last** time it was invoked. Commonly used for search bar autocomplete inputs and window resize events.

```javascript
function debounce(func, delay) {
  let timerId;
  return function (...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// Usage
const handleSearch = debounce((query) => {
  console.log("Fetching API for:", query);
}, 300);
```

---

### Q87: What is throttling?
**Answer:**
**Throttling** is an optimization pattern that guarantees a function is called **at most once in a given time interval**, regardless of how many times the trigger event fires. Ideal for scroll handlers, infinite scroll, and mouse move tracking.

```javascript
function throttle(func, interval) {
  let lastTime = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastTime >= interval) {
      lastTime = now;
      func.apply(this, args);
    }
  };
}

// Usage
window.addEventListener("scroll", throttle(() => {
  console.log("Scroll position checked");
}, 200));
```

---

### Q88: Debounce vs Throttle
**Answer:**

| Feature | Debounce | Throttle |
| :--- | :--- | :--- |
| **Execution** | Waits for quiet period after last event | Executes periodically at regular fixed intervals |
| **Reset Behavior**| Timer resets on every trigger | Timer continues steadily |
| **Primary Use Case**| Auto-complete search inputs, form validation | Infinite scrolling, window resizing, game tick |

---

### Q89: What is memoization?
**Answer:**
**Memoization** is an optimization technique where expensive function call results are cached based on input parameters. If the same input is received again, the cached result is returned directly without recomputing.

```javascript
function memoize(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const slowSquare = (n) => {
  // Heavy computation
  return n * n;
};
const fastSquare = memoize(slowSquare);
```

---

### Q90: What is garbage collection?
**Answer:**
JavaScript features automatic memory management via a **Garbage Collector (GC)**.
The primary algorithm used by modern engines (like V8) is **Mark-and-Sweep**:
1. Starts from root objects (Global object, current execution stack).
2. Traverses and "marks" all reachable objects.
3. "Sweeps" and frees memory of all unreachable (unmarked) objects.

---

### Q91: Stack vs Heap
**Answer:**
- **Stack:** Stores primitive values and function call execution contexts. Fast access, fixed size, automatically managed by OS.
- **Heap:** Stores complex reference types (objects, arrays, functions). Dynamically allocated, variable size, managed by Garbage Collector.

---

### Q92: What causes memory leaks in JavaScript?
**Answer:**
1. **Accidental Global Variables:** Missing `var`/`let`/`const` binds variables to `window`.
2. **Forgotten Timers & Callbacks:** `setInterval` holding references to outer variables.
3. **Uncleared Event Listeners:** DOM elements removed without detaching listeners.
4. **Out of DOM References:** Retaining references to removed DOM elements in JS variables.
5. **Improper Closures:** Closures retaining large unwanted objects in memory.

---

### Q93: ES Modules (ESM) vs CommonJS (CJS)
**Answer:**

| Feature | ES Modules (ESM) | CommonJS (CJS) |
| :--- | :--- | :--- |
| **Syntax** | `import` / `export` | `const x = require()` / `module.exports` |
| **Loading** | Asynchronous / Static analysis | Synchronous / Runtime execution |
| **Tree Shaking** | Supported (eliminates unused code) | Not easily supported |
| **Top-level `await`**| Supported | Not supported in standard CJS |
| **Standard** | Native ECMAScript standard (Browsers + Node) | Node.js legacy standard |

---

### Q94: `require()` vs `import`
**Answer:**
- `require()`: Dynamic, can be called conditionally inside `if` statements, loads modules synchronously.
- `import`: Static, must be at top-level file scope (except `import()`), evaluated during compilation phase before code runs.

---

### Q95: What is Strict Mode (`"use strict"`)?
**Answer:**
Opt-in mode introduced in ES5 that enforces cleaner code:
1. Prevents accidental global variable creation.
2. Throws errors on silent failures (e.g. assigning to read-only property).
3. Disallows duplicate parameter names in functions.
4. Makes `this` inside plain functions `undefined` instead of `window`.
5. Disallows `with` statement and `eval` scope leaks.

---

### Q96: What is a prototype?
**Answer:**
In JavaScript, every object has an internal link to another object called its **prototype** (`[[Prototype]]`, accessible via `Object.getPrototypeOf(obj)` or `__proto__`). The prototype object contains shared properties and methods inherited by all instances.

---

### Q97: What is the Prototype Chain?
**Answer:**
When accessing a property on an object, if it's not found on the object itself, JS searches its prototype. If not found there, it searches the prototype's prototype, moving up until it reaches `Object.prototype` (whose prototype is `null`). This hierarchy is the **Prototype Chain**.

---

### Q98: Prototypal Inheritance
**Answer:**
A mechanism where objects inherit properties and methods directly from other objects without traditional classes.

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hello, I am ${this.name}`;
};

const user = new Person("Sangam");
console.log(user.greet()); // "Hello, I am Sangam"
```

---

### Q99: Class vs Prototype
**Answer:**
ES6 `class` is **syntactic sugar** over JavaScript's existing prototype-based inheritance.
Under the hood, classes still use prototype chains, constructor functions, and `Object.create()`. Classes provide clearer syntax for constructors, `extends`, `super()`, and static methods.

---

### Q100: What are Generators?
**Answer:**
**Generators** are special functions defined with `function*` that can pause execution midway and resume later using the `yield` keyword. Calling a generator returns a **Generator Object** (which conforms to both Iterable and Iterator protocols).

```javascript
function* idGenerator() {
  let id = 1;
  while (true) {
    yield id++;
  }
}

const gen = idGenerator();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
```

---

### Q101: What are Iterators?
**Answer:**
An **Iterator** is an object with a `next()` method that returns an object with two properties:
`{ value: any, done: boolean }`.
An object is **iterable** if it defines a method at `[Symbol.iterator]`.

```javascript
const customIterable = {
  items: [10, 20],
  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => {
        if (index < this.items.length) {
          return { value: this.items[index++], done: false };
        }
        return { done: true };
      }
    };
  }
};

for (const val of customIterable) {
  console.log(val); // 10, 20
}
```

---

### Q102: What is `Symbol`?
**Answer:**
A primitive data type introduced in ES6 that produces a **guaranteed unique and immutable** identifier. Often used as private-like object property keys to avoid property name collisions.

```javascript
const id1 = Symbol("id");
const id2 = Symbol("id");
console.log(id1 === id2); // false

const user = {
  name: "Sangam",
  [id1]: 12345
};
// Hidden from Object.keys() and for...in loops
console.log(Object.keys(user)); // ["name"]
```

---

### Q103: What is `BigInt`?
**Answer:**
A primitive data type introduced in ES2020 for safely representing integers larger than `Number.MAX_SAFE_INTEGER` ($2^{53} - 1 = 9007199254740991$). Created by appending `n` to an integer literal or via `BigInt()`.

```javascript
const maxSafe = Number.MAX_SAFE_INTEGER;
console.log(maxSafe + 1 === maxSafe + 2); // true (Accuracy lost!)

const big1 = 9007199254740991n;
const big2 = big1 + 2n;
console.log(big2); // 9007199254740993n (Accurate)
```

---
*Created for interview preparation and quick revision.*
