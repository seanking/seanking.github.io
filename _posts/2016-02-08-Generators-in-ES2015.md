---
layout: post
title: "Generators in ES2015"
---

# Generators in ES2015

Generators are a new feature provided by the [ES2015 Language Specification](http://www.ecma-international.org/ecma-262/6.0/#). A generator is a method that can be exited and re-entered multiple times without losing state. The examples below will step through creating basic and complex generators.

## Basic Generator

A generator must be declared using _function*_ and contain at least one _yield_ statement.

```javascript
function* newIterator() {
  yield 1;
  yield 2;
}
```

Calling a generator method does not execute the body but instead returns a _Generator_ object.

```javascript
function* newIterator() {
  ...
}

const iter = newIterator();
```

The Generator object supports the [iterable and iterator protocols](https://github.com/seanking/es2015-iterators). Calling the object's _next_ method will execute the function until the first _yield_ statement. The expression following the _yield_ will be returned. The following iterations will continue from the previous _yield_ statement and execute until the next _yield_ statement.

In the following example, iterating over the generator will produce 1 and then 2.

```javascript
function* newIterator() {
  yield 1;
  yield 2;
}

const iter = newIterator();

console.log(iter.next()); // prints { value: 1, done: false }
console.log(iter.next()); // prints { value: 2, done: false }
console.log(iter.next()); // prints { value: undefined, done: true }
```
## Complex Generators

### Internal State

Generators have the ability to maintain state within the function. This allows for complex algorithms that can produce results on demand.

In the following example, the generator function uses a _for_ loop to increment the state of _i_. The internal state enables the function return the new value of _i_ for each iteration.

```javascript
function* newIterator() {
  for (let i = 0; i < 2; i++) {
    yield i;
  }
}

const iter = newIterator();

console.log(iter.next()); // prints { value: 1, done: false }
console.log(iter.next()); // prints { value: 2, done: false }
console.log(iter.next()); // prints { value: undefined, done: true }
```

### Modification of Internal State

The internal state of a generator can be modified by passing a value into the iterator's _next_ method. Any value passed into the _next_ method will be returned from the _yield_ statement.

In the following example, a boolean flag is passed into the iterator's _next_ method to reset the count.

```javascript
function* newIterator() {
  for (let i = 0; i < 3; i++) {
    const reset = yield i;

    if (reset) {
      i = -1; // reset for loop
    }
  }
}

const iter = newIterator();

console.log(iter.next());     // prints { value: 0, done: false }
console.log(iter.next());     // prints { value: 1, done: false }
console.log(iter.next(true)); // prints { value: 0, done: false }
console.log(iter.next());     // prints { value: 1, done: false }
console.log(iter.next());     // prints { value: 2, done: false }
console.log(iter.next());     // prints { value: undefined, done: true }
```

## Future

Generators appear to be a fundamental part of the language moving forward. The [Async-Await](http://tc39.github.io/ecmascript-asyncawait/) proposal for ES7 leverages generators to improve the language's support for writing asynchronous JavaScript.

As of this post, generators are supported in [nodejs](https://nodejs.org/en/) and in some major browsers. Unlike most ES6 features, [babel's polyfill]([polyfill](https://babeljs.io/docs/usage/polyfill)) must be used instead of the transpiler. As with most polyfills, tread carefully when adding any dependency to your code.
