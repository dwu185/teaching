---
layout: bb-reveal-single-slide
title: ES6 Iterable
controlsFlag: true
---

# ES6 Iterable

---

## Overview
- Iterable
- Iterator
- Operations that Use Iteration
- Make Your Own Iterable
- Summary

note:
ES6 did not add any new Class called Iterable or Iterator. It also didn't add any new types called Iterable or Iterator. Iterable and Iterator are simply protocols.

---

# Iterable

---

## What is an Iterable?
- Anything that satisfies the Iterable Protocol is an iterable
- Iterable Protocol:
    - `[Symbol.iterator]()`: returns an iterator
- Built-in iterable data structures
    - Arrays, Strings, Maps, Sets
- Plain objects are NOT iterable
- WeakMap and WeakSet are NOT iterable
```javascript
const arr = [1, 2, 3];
console.log(typeof arr[Symbol.iterator]);
```
```javascript
const obj = {};
console.log(typeof obj[Symbol.iterator]);
```

note:
`Symbol.iterator` is a well known symbol: a static property on Symbol class. We can use it to access or modify internal language behavior, in this case, iteration.
What is the iterable protocol? An iterable must have a key Symbol.iterator which is a method, that when invoked, returns an iterator. In other words `Symbol.iterator` is an iterator factory
Why is object not iterable? If object is iterable, you are iterating over properties. But iteration is designed for iterating over data. If your object is designed to hold data, then you should use Map.

---

# Iterator

---

## Iterator
### What is an Iterator?
- General Definition:
    - A pointer that traverses data in a container and returns one element at a time
- JavaScript Definition:
    - Anything that satisfies the Iterable Protocol is an iterable

---

## Iterator
### What is the Iterator Protocol?
- `.next()`: returns an object with two properties `done` and `value`
    - `done` is a boolean indicating whether iteration is finished
    - `value` can be any type and it is the current value of the iteration
```javascript
const arr = [1, 2];
const arrIterator = arr[Symbol.iterator]();
console.log(arrIterator.next());
console.log(arrIterator.next());
console.log(arrIterator.next());
```

---

# Operations that Use Iteration

note:
Iteration is nice, but accessing the iterator through the iterator symbol and then access each value by calling .next is pretty cubersome.
Luckily ES6 introduced many language constructs that process iterables seamlessly, which makes this feature so much more powerful.

---

## Operations that Use Iteration
### Destructuring, Spread, Array.from
- Destructure an iterable to elements in an array
```javascript
const [a, b] = 'hi';
console.log(`a: ${a}, b: ${b}`);
```
- Spread an iterable into an array
```javascript
const arr = [1, 2, 3];
const arrExpanded = [0, ...arr, 4];
console.log(`Expanded array: ${arrExpanded}`);
```
- Array.from() converts iterable to an array
```javascript
const arr = Array.from('hi!');
console.log(`Array: ${arr}`);
```

---

## Operations that Use Iteration
### for...of
- `for...of` loops through an iterable
```javascript
const arr = [1, 2, 3];
for (const i of arr) {
    console.log(i);
}
```

note:
Array is iterable but we already have regular for loop and forEach.
So why should we use for...of to iterate through an array?

---

### `for` loop vs. `forEach()` vs. `for...of`
- for loop: can break/continue
```javascript
const numbers = [1, 2, 3, 4, 5];
let sum = 0;
for (let i = 0; i < numbers.length; ++i) {
	if (sum + numbers[i] > 10)
		break;
	else
		sum += numbers[i];
}
```
- forEach: more concise
```javascript
let sum = 0;
numbers.forEach( (num) => {
	if (sum + num <= 10)
		sum += num;
});
```
- for...of: combines two advantages
```javascript
let sum = 0;
for (const num of numbers) {
	if (sum + num > 10)
		break;
	sum += num;
}
```

---

## Operations that Use Iteration
### ES6 Map and Set
- Map and Set constructors can take an iterable as input to populate the container
    - WeakMap and WeakSet constructors work similarly
```javascript
const map = new Map([
    ['key1', 'val1'],
    ['key2', 'val2']
]);
console.log(map);
```
```javascript
const set = new Set('123');
console.log(set);
```
- Map and Set are iterables themselves. You can shallow clone a map/set using its constructor
```javascript
const mapClone = new Map(map);
```

---

# Make You Own Iterable

---

## Make You Own Iterable
```javascript
class MyIterable {
    constructor (str) {
        this.str = str;
        this.itrIndex = 0;
    }
    [Symbol.iterator]() {
        const next = () => {
            if (this.itrIndex < this.str.length) {
                this.itrIndex++;
                const value = this.str.slice(0, this.itrIndex);
                return { value }; // Omitted done
            }
            return { done: true }; //Omitted value
        }
        return {next};
    }
}
const myIterable = new MyIterable('abc');
for (const i of myIterable) {
    console.log(i);
}
```

note:
Simplified return value of next method: can omit `done` if it is false, can omit `value` if it is undefined

---

## Make You Own Iterable
### Iterable is also a Iterator
```javascript
class MyIterable {
    constructor (str) {
        this.str = str;
        this.itrIndex = 0;
    }
    [Symbol.iterator]() {
        return this;
    }
    next() {
        if (this.itrIndex < this.str.length) {
            this.itrIndex++;
            const value = this.str.slice(0, this.itrIndex);
            return { value };
        }
        return { done: true };
    }
}
const myIterable = new MyIterable('abc');
for (const i of myIterable) {
    console.log(i);
}
```

note:
Pattern to simplify the implementation of an iterable: Make the iterable also the iterator. 
const myIterable2 = new MyIterable('abc');
myIterable2.next();

---

## Summary
- Something is iterable if it has Symbol.iterator implemented as a iterator factory
- Something is iterator if it has next method returning {done, value} object with each call
- Coming up: It is much easier to implement an iterable via a generator
