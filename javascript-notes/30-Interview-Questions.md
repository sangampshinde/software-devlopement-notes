## 1.What are the differences between var, let, and const in JavaScript?

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
