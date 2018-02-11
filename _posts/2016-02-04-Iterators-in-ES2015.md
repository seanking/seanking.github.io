---
layout: post
title: Iterators in ES2015/ES6
description: Introduction into Iterators in ES2015/ES6
redirect_from: news/2016/2/1/iterators-generators-and-async-programming-part-1/
---

# Iterators in ES2015
Over the last 20 years, JavaScript hasn't had native support for the [iterator pattern](https://en.wikipedia.org/wiki/Iterator_pattern). The [ES2015 Language Specification](http://www.ecma-international.org/ecma-262/6.0/) changed this by introducing iteration protocols.

The following examples demonstrate how to iterate over iterable objects (e.g.: Array, Map, Set, String, etc...) and define iterable objects using the iteration protocols.

### ES5

Developers coming from other programming languages are often surprised by the lack iteration support in JavaScript. Prior to the introduction of iterators, the simplest way to iterate over an array was with a traditional for loop.

```javascript
const arr = [1, 2, 3];

for (let i = 0; i < arr.length; i++) {
  const number = arr[i]; // extract the number from array
  console.log(number); // prints 1, 2, 3
}
```

Though it may be common for developers to use a _for...in_ loop to iterate over an object instead of the traditional for loop, the _for...in_ loop only iterates over the enumerable properties and order is not guaranteed.

### ES2015

Iterating over an aggregate object has been simplified in ES2015. A _for...of_  loop was introduced to iterate over objects that implement the iterable protocol. In the following example, the _for...of_  loop iterates over an array.

```javascript
const arr = [1, 2, 3];

for (const i of arr) {
  console.log(i); // prints 1, 2, 3
}
```

## ITERATION PROTOCOLS

The ES2015 language specification introduced an iterable and iterator protocols to support iterating over objects.

### ITERABLE PROTOCOL

An iterable must implement the iterable protocol. This protocol requires the object implement an _@@iterator_ method that returns an iterator.

This sounds simple, but instead of defining a method using _@@iterator_, the method must be defined using the _Symbol.iterator_ with block notation.

```javascript
class Aggregate {
  [Symbol.iterator]() { // implements the iterable protocol
    // returns an object that implements the iterator protocol
  }
}
```

### ITERATOR PROTOCOL

An iterator must implement the iterator protocol. This protocol requires the object implement a next method. The next method must return an object containing two properties (value and done).

**value:** can contain any object  
**done:** a boolean flag identifying the completion of the iteration


#### CUSTOM ITERATOR

In the following example, the _Aggregate_'s _@@iterator_ method is updated to return an iterator. The iterator will produce a sequence of values.

```javascript
class Aggregate {
  constructor() {
    this.values = [1, 2, 3]; // internal collection
  }

  [Symbol.iterator]() { // implements the iterable protocol
    let index = 0;
    return {  // returns an iterator protocol
      next: () => {
        const value = this.values[index];
        const done = index++ >= this.values.length;
        return { value, done };
      }
    };
  }
}
```

In the following example, the values returned from the the _Aggregate_'s iterator are logged to the console. The first, second, and third objects returned contain the values from the _Aggregate_'s internal array and the done flag set to false. The last object returned has the done flag set to true, signaling the iteration is over.

```javascript
const agg = new Aggregate();

const iter = agg[Symbol.iterator]();

console.log(iter.next()); // prints {value: 1,done: false}
console.log(iter.next()); // prints {value: 2,done: false}
console.log(iter.next()); // prints {value: 3,done: false}
console.log(iter.next()); // prints {value: undefined,done: true}
```

Now that the _Aggregate_ class is iterable, the _for...of_  loop can be used to iterate the object without exposing its inner collection.

```javascript
const agg = new Aggregate();

for (const i of agg) {
  console.log(i); // prints 1, 2, 3
}
```

## SUPPORT

As of this post, [nodejs](https://nodejs.org) provides support for the iterable and iterator protocols, but only a few browsers support the protocols. However, this doesn't mean that developers cannot use these features on the client-side now.  Polyfills and trans-compilers like [Babel](https://babeljs.io/) can enable these features now.
