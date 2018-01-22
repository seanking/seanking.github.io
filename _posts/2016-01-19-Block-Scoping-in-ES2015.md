---
layout: post
title: "Block Scoping in ES2015/ES6"
---

# Block Scoping with ES2015

This project tests two functions. Both functions are identical except for their scoping of variables.  The function scope example uses a for-loop with a _var_ statement. The block scope example uses a for-loop with a  _let_ statement.

### Ex: Function Scope
The mix of function scoping and closures can introduce bugs that even the most seasoned JavaScript developer might overlook.

The variable _i_ is scoped to the function in this example. The function scoped variable will result in each _print_ closure having the same reference to _i_. Since the _print_ function is executed on a delay, the variable _i_ will be updated to 3 before the _print_ functions are executed.

```javascript
function countWithFunctionScope() {
  for (var i = 0; i < 3; i++) {
    setTimeout( function print() {
      console.log(i); //prints 3, 3, 3
    }, 1000);
  }
}
```

### Ex: Block Scope
Updating the previous example to use a _let_ statement will provide different results. Since the variable _i_ is block scoped, each _print_ closure will have a different reference to _i_.  This will result in the printing of 0, 1, and 2.

```javascript
function countWithBlockScope() {
  for (let i = 0; i < 3; i++) {
    setTimeout( function print() {
      console.log(i); //prints 0, 1, 2
    }, 1000);
  }
}
```

### Conclusion
JavaScript style guides (ex: Airbnb) have quickly been updated to enforce the use of block scoping, instead of function scoping. I recommend doing the same. Limiting the scope of a variable can only help reduce the cognitive load required to understand an algorithm.
