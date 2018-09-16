---
layout: bb-reveal-single-slide
title: ES6 Iterable
controlsFlag: true
---

# ES6 Generator

---

## Overview
- What is a Generator?
- Use Cases
    - Generator as Data Producer
    - Generator as Data Consumer

---

# What is a Generator?

---

## What is a Generator?
### Generator vs. Regular Function
```javascript
function* gen () {
    console.log('start');
    yield;
    console.log('end');
}
console.log(typeof gen);
```
- Unlike regular function...
    - Generator does not need to run to completion
    - Generator can be suspended and resumed later

note:
In the example below, the generator looks very similar to a normal function. However, there's a big difference between them.
A javascript function always runs to completion when it is invoked. Before generator, there's no mechanism to control the execution of a function body. Generator gives some control back to the caller. It can be paused somewhere in the function body and resume later.

---

## What is a Generator?
### Generator Function Overview
```javascript
function* gen () {
    console.log('start');
    yield;
    console.log('end');
}
```
- Three New Keywords
    - `function*`: indicates that the function is a generator
    - `yield`:  determines where to pause; can receive input and generate output
    - `yield*`: let you use another iterable or generator
- Other special statements
    - `return`
    - `throw`

---

## What is a Generator?
### Generator Object Overview
- Terms: generator ('aka generator function') vs. generator object
- When a generator is invoked, it returns a generator object
```javascript
function* gen () {
    console.log('start');
    yield;
    console.log('end');
}
const generatorObj = generatorFunction();
```
- The generator object:
    - Can control the execution of its generator function
    - Can pass input into or receive output from its generator function
- Three APIs to control execution
    - `generatorObj.next(val)`
    - `generatorObj.return(val)`
    - `generatorObj.throw(err)`

note:
A Generator is also known as generator function, which is different from generator object.
...
Next, we will cover how does a generator function and its generator object interact with each other by going through a few useful use cases.

---

# Use Cases

---

# Generator Object as Data Producer

---

## Generator Object as Data Producer
### How?
- Four ways a generator function can generate data:
    - `yield val;`
    - `return val;`
    - `throw err;`
    - `yield* iterator;`
- A generator object can access those data with `generatorObj.next()`
    - returns data as an object containing `done` and `value`
- Generator object satisfies iterator protocal
    - Iterator is a commonly used data producer
- Generator object also satisfies iterable prototcol
    - The importance of this will be covered later

---

## Generator Object as Data Producer
### `yield val` and `generatorObj.next()`
- A generator function with two `yield`
- next() returns `{value: val: done: false}`
```javascript
function* gen () {
    console.log('start');
    yield 'a';
    console.log('middle');
    yield 'b';
    console.log('end');
}
const generatorObj = gen();
generatorObj.next();
// What happens if we call .next again?
```
note:
Let's start with how `yield` statement in a generator function and .next call on a generator object work together to produce a sequence of data. 
first .next call: starts the execution of the generator function until it processed `yield 'a'`, then it will pause. So 'start' is logged and this call return an object with {value: 'a', done: false}...

---

## Generator Object as Data Producer
### `return val;` and `generatorObj.next()`
- A generator function with a `return` statement
- next() returns `{value: val: done: true}`
```javascript
function* gen() {
    yield 1;
    return 2;
}
const generatorObj = gen();
console.log(generatorObj.next());
console.log(generatorObj.next());
```

---

## Generator Object as Data Producer
### `throw err;` and `generatorObj.next()`
- A generator function with a `throw` statement
- next() throws the err
```javascript
function* gen() {
    throw Error('test');
}
const generatorObj = gen();
generatorObj.next();
```

---

## Generator Object as Data Producer
### `yield* iterable` and `generatorObj.next()`
- `yield*`: Use iterable as data source within a generator
```javascript
function* gen() {
    yield* [1, 2];
    yield 3;
}
const generatorObj = gen();
console.log(generatorObj.next());
console.log(generatorObj.next());
console.log(generatorObj.next());
```
- Equivalent implementation using `yield` only
```javascript
function* gen() {
    for (const i of [1, 2]) {
        yield i;
    }
    yield 3;
}
```
- generator object is iterable: `yield* generatorObj`

note:
`yield* generatorObj`: Can recursively call a generator

---

## Generator Object as Data Producer
### Iterable
- Generator object is both an iterable and an iterator
- Many language constructs that consumes data from iterables:
    - Spread
    - Set, Map, WeakMap, WeakSet constructors
    - destructuring
    - for...of
    - etc.
```javascript
function* gen() {
    yield 1;
    yield 2;
    return 3;
}
const generatorObj = gen();
[...generatorObj]; // Do no do this if stream is infinite
```
- Returned value will be ignored: `done` is true

note:
These language constructs ignore the return value if iteration is done. You can access the return value using .next().

---

## Generator Object as Data Producer
### An Easier way to Implement Iterable
- Without using generator...
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
            return { done: false, value };
        }
        return {done: true};
    }
}
const growingStr = new MyIterable('abc');
for (const i of growingStr) {
    console.log(i);
}
```
- Use generator...
<!--DIV-LEFTCOLUMN-->
```javascript
class MyIterable {
    constructor (str) {
        this.str = str;
        this.itrIndex = 0;
    }
    *[Symbol.iterator] () {
        while (this.itrIndex < this.str.length) {
            this.itrIndex++;
            yield this.str.slice(0, this.itrIndex);
        }
    }
}
const growingStr = new MyIterable('abc');
for (const i of growingStr) {
    console.log(i);
}
```
<!--DIV-RIGHTCOLUMN-->
```javascript
function* growingStrGen (str) {
    let grow = '';
    for (const c of str) {
        grow += c;
        yield grow;
    }
}
const growingStr = growingStrGen('abc');
for (const i of growingStr) {
    console.log(i);
}
```

---

## Generator Object as Data Producer
### Benefits
- Why do we use a generator object to produce data?
    - Alternative: a function turning `'abc'` to `['a', 'ab', 'abc']`
- Benefits of Generator Object as Data Producer
    - Memory efficiency: produce on demand
    - Stream of data that has infinite size

note:
If the size of the input string is much larger and the caller only needs output with length less than 10: a lot of memory is wasted.
Given a generator, you can easily generate an array of all data with spread.

---

# Generator Object as Data Consumer

---

## Generator Object as Data Consumer
### How?
- A generator object can pass data into a generator function in three ways:
    - `generatorObject.next(value)`
    - `generatorObject.return(value)`
    - `generatorObject.throw(err)`
- The generator function consumes the data via `yield`

---

## Generator Object as Data Consumer
### `generatorObject.next(value)` and `yield`
- Invoke `next(value)` with a single argument `value` will pass it into the generator function
- `value` will be assigned to the currently suspended `yield`
```javascript
function* gen() {
    console.log('start');
    console.log(yield);
    console.log(yield);
    console.log('end');
}
const generatorObj = gen();
```
- First `next()` call only starts the generator function and suspends at the first `yield`
- After the first `next()`, generator object can start receiving data
- Second `next()` call passes value to the suspended `yield` and executes until it suspends at the second `yield`
- Third `next()` call passes value into the second suspended `yield` and executes until the end of the function

note:
Let's start with how genObj.next and yield expression inside the generator function works together to consume data.
Two main differences compare to generator object as a data producer:
1. yield is not followed by operand: we are not producing data
2. .next has a value input, ignore the returned object

---

## Generator Object as Data Consumer
### `next()` Summary
- First `next()`: do not need to pass in value
    - Starts the generator, executes until the first `yield`
    - Returns `{value: <operand of suspended yield>, done: false}`
- `next(val)` calls after the frist
    - The currently suspended `yield` is set to `val`
    - Executes until the next `yield`, `return` or `throw`
        - `yield val`: returns `{value: val, done: false}`
        - `return val`: returns `{value: val, done: true}`
        - `throw err`: throws err
- Producing vs. Consuming
    - The nth `yield` will produce data at the nth `next()`
    - The nth `yield` will consume data at the (n+1)th `next(val)`

---

## Generator Object as Data Consumer
### `return val` and `throw err`
- `return` and `throw` in generator function acts the same whether the generator object is used as data consumer or producer
```javascript
function* gen() {
    console.log('start');
    console.log(yield);
    return 'end';
}
const generatorObj = gen();
```
```javascript
function* gen() {
    console.log('start');
    console.log(yield);
    throw Error('err!');
}
const generatorObj = gen();
```

note:
return: second `next` will get to the end of the function and return {done: true, value: `end`}
throw: second `next` will throw the error

---

## Generator Object as Data Consumer
### `generatorObject.return(value)`
- Generator object can terminate the generator with `return()`
```javascript
function* gen() {
    while (true) {
        const val = yield;
        console.log(val);
    }
}
const generatorObj = gen();
generatorObj.next(); //start
generatorObj.next('a');
generatorObj.return('b');
```

note:
Before calling `return()`, execution is pausing at the yield statement.
When `return()` is called, the generator function terminates. You can imagine the yield statement getting replaced by the return statement.

---

## Generator Object as Data Consumer
### `generatorObject.throw(err)`
- `throw()` also terminates generator by throwing an error
```javascript
function* gen() {
    while (true) {
        const val = yield;
        console.log(val);
    }
}
const generatorObj = gen();
generatorObj.next(); //start
generatorObj.next('a');
generatorObj.throw(Error('oops!'));
```
```javascript
function* gen() {
    try {
        while (true) {
            const val = yield;
            console.log(val);
        }
    }
    catch (err) {
        console.log('Error: ' + err);
    }
}
```

note:
You can imagine the suspended yield statement getting replaced by the throw statement.
Protection the generator from crashing by wrapping code block containing yield in try-catch
