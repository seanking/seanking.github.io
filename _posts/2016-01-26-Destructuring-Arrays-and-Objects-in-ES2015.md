---
layout: post
title: Destructuring Arrays and Objects in ES2015/ES6
description: Introduction into Destructuring Arrays and Objects in ES2015/ES6
redirect_from: news/2016/1/24/destructing-arrays-and-objects-in-es2015/
---

## Destructuring Arrays and Objects in ES2015

ES2015 introduced destructuring, a syntactic sugar for extracting data from arrays or objects.  This feature has not gained the attention of other features provided by ES2105, but it may do the most to improve the verbosity of JavaScript. The [destructuring_spec.js](test/destructuring_spec.js) has tests for the examples listed below.

### DESTRUCTURING ARRAYS

#### EXAMPLE: ES5

Extracting data from an array can be verbose in ES5. The following example requires multiple lines to extract values from an array.

```javascript
var array = [1, 2, 3];

var first  = array[0];
var second = array[1];
var third  = array[2];

console.log(first);  // prints 1
console.log(second); // prints 2
console.log(third);  // prints 3
```

#### EXAMPLE: ES2015

Using the destructuring assignment the values of the array can be extracted in one line. This will allow developers to be more terse and reduce defects caused by copying and pasting.  

```javascript
const array = [1, 2, 3];

const [first, second, third] = array;

console.log(first);  // prints 1
console.log(second); // prints 2
console.log(third);  // prints 3
```

#### EXAMPLE: PARTIAL DESTRUCTURING

Sometimes there is an item in an array that is unneeded. The following example demonstrates how to extract the first and third indices, while not extracting the second index.

```javascript
const array = [1, 2, 3];

const [first,, third] = array;

console.log(first); // prints 1
console.log(third); // prints 2
```

#### EXAMPLE: PAIRS AND TUPLES

Destructuring arrays provide developers a nice way to handle functions that return multiple values. The following example has a function that returns a pair and the returned array is destructured into two variables.

```javascript
function f() {
    return [1, 2];
}

const [first, second] = f();

console.log(first);  // prints 1
console.log(second); // prints 2
```

### DESTRUCTURING OBJECTS

#### EXAMPLE: ES5

Extracting data from an object can be a tedious and error prone activity. The following example takes several lines to extract data from a JSON object.

```javascript
var json = { one: 1, two: 2, three: 3 };

var one = json.one;
var two = json.two;
var three = json.three;

console.log(one);   // prints 1
console.log(two);   // prints 2
console.log(three); // prints 3
```

#### EXAMPLE: ES2015

Destructuring simplifies the extraction of data from a JSON object. The following example extracts the data from the object in one line, as opposed to three from the previous example.

```javascript
const json = { one: 1, two: 2, three: 3 };

const { one, two, three } = json;

console.log(one);   // prints 1
console.log(two);   // prints 2
console.log(three); // prints 3
```

#### EXAMPLE: COMPLEX OBJECT

JSON objects are seldom as flat as the object in the prior example. The following example demonstrates how to extract data from a more complex object.

```javascript
const json = { numbers: { one: 1, two: 2, three: 3 } };

const { numbers: { one } } = json;

console.log(one); // prints 1
```

### CONCLUSION

As of this post, most evergreen browsers and [node.js](https://nodejs.org/en/) (using the --harmony_destructuring flag) support destructuring. This means that both the client side and server side's RESTful services can benefit from this destructuring now. To support legacy browsers or services, polyfills or trans-compilers like [Babel](https://babeljs.io) can be used.
