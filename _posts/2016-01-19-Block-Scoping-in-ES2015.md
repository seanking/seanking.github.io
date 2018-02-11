---
layout: post
title: Block Scoping in ES2015/ES6
description: Introduction into Block Scoping in ES2015/ES6 (var, let, and const)
redirect_from: news/2016/1/13/let-and-cost-es2015/
---

## Block Scoping in ES2015/ES6

### Introduction

Variables in JavaScript are function or global scoped. This can come as a surprise to developers that traditionally expect block scoping from a C-like syntax. To the delight of a lot of developers, the ES2015 language specification introduce let and const statements to provide block scoping. The following examples aim to demonstrate the benefits of block scoping in JavaScript. 


### EX: Function Scopeing

In the following example, the variable i is scoped to the count function, not the for-loop block. This means that the i variable can be accessed outside the for-loop as demonstrated in the last console.log statement. 

```javascript
function count() {
  for (var i = 0; i <= 3; i++) {
    console.log(i); //prints 0, 1, 2, 3
  }
  console.log(i); //prints 4
};
```

### EX: Block Scoping ES2015

Updating the previous example to use let instead of var will result in a different outcome. The for-loop will behave the same as in the prior example, but the final reference to i will result in a ReferenceError. The error occurs because the i variable has not declared outside the for-loop block. 

```javascript
function count() {
  for (let i = 0; i <= 3; i++) {
    console.log(i); //prints 0, 1, 2, 3
  }
  console.log(i); //ReferenceError
};
```

### EX: Subtle Error

Do you see why the following function will print 3, 3 and 3?

```javascript
function countWithPause() {
  for (var i = 0; i < 3; i++) {
    var milliseconds = 100;

    setTimeout(function print() {
      console.log(i); //prints 3, 3, 3
    }, milliseconds);
  }
};
```

Don't worry if you cannot. The mix of function scoping and closures can introduce bugs that even the most seasoned JavaScript developer might overlook.

In the previous example, the variable i is scoped to the function. The function scoped variable will result in each print closure having the same reference to i. Since the print function is executed on a delay, the variable i will be updated to 3 before the print functions are executed.


### EX: Subtle Error Resolved

Updating the previous example to use a let statement will provide different results. Since the variable i is block scoped, each print closure will have a different reference to i.  This will result in the printing of 0, 1, and 2.

```javascript
function countWithPause() {
  for (let i = 0; i < 3; i++) {
    var milliseconds = 100;

    setTimeout(function print() {
      console.log(i); // prints 0, 1, 2
    }, milliseconds);
  }
};
```

### Constants

The const statement declares a read-only reference to a value. Like declaring a variable with the let statement, constants are also blocked-scoped. The following example demonstrates the behavior of a constant. 

```javascript
const GREETING = 'hello world!';
console.log(GREETING); // prints 'hello world!'

GREETING = 'Hola Mundo'; // overwriting the value fails
console.log(GREETING); // prints 'hello world!'
OBJECTS
const JSON = { foo: 1 };
JSON = { bar: 2 }; // overwriting the object fails
console.log(JSON) // { foo: 1 };

JSON.foo = 100; // overwriting an attribute is possible
console.log(JSON) // prints { foo: 100 };
```


### Future

As of this post, there are not any browsers that fully support let and const. Let  is not supported by any major browser. Some browsers support const, while others will let you reassign its value of a constant (Say what‽). Using trans-compilers like [Babel](https://babeljs.io/) will provide support for the let statement and a consistent behavior for the const statement.

JavaScript style guides (ex: [Airbnb](https://github.com/airbnb/javascript)) have quickly been updated to enforce the use of block scoping, instead of function scoping. I recommend doing the same. Limiting the scope of a variable can only help reduce the cognitive load required to understand an algorithm. 

The scoping examples can be viewed on [GitHub](https://github.com/seanking/es2015-scoping). Please clone the repository and experiment with block scoping. Enjoy!