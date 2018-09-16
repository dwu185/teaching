---
layout: bb-reveal-single-slide
title: ES6 Containers
controlsFlag: true
---

# ES6 Containers

---

## Objectives

- Background
- `for...of`
- `Map`
- `Set`
- `WeakMap`
- `WeakSet`
- Iterables

---

## Background: The Old Containers

* JS used to have two categories of containers
  * Arrays - Typically used for a collection of similar items
  * Objects - Allowing key-value matching using the object's properties
* Both are regular objects under the hood, with keys being `string`
  * Or `Symbol` since ES6
* ES6 introduced traditional Data Structures to contain data

---

## Background: The Old Iteration Techniques

* Iterating through arrays could be done with `Array` methods `map`, `forEach`, `reduce`, `filter`, or traditional loops (`for`, `while`, `do...while`) depending on what we are trying to do
* Iterating through objects was done via `for...in` or by using `Object` methods such as `Object.keys()`
  * These would iterate through enumerable properties of the object
  * In the case of `for...in`, all enumerable properties would be traversed
    * With no regard of where in the prototype chain they were found
* ES6 introduced the idea of **Iterable Objects**, which can be more easily traversed than traditional object
  * They also allow the user to decide *how* to iterate those objects

---

## `for...of`

* ES6 Introduces `for...of`
* It allows iterating through the values of any **iterable** item
* Arrays and strings are iterable
  * So are the new `Map` and `Set`
* It is possible to make other objects iterables (see later slide)

```javascript
const str = "Hello World";
const arr = [1, 2, 3, 4, 5];

for (const n of arr) {
    console.log(n);
}

for (const l of str) {
    console.log(l);
}
```

---

## `Map`

* `Map` is a container holding key-value pairs
* Unlike regular Objects, keys can be any type (both objects and primitives)
* `Map` is also capable of using `NaN` as a key
* `Map` iteration with `for...of` is done via an array with `[key, value]`
  * Example to follow

---

## Differences Between `Map` and `Object`

* We can get the size of a `Map` with `.size`
* Properties of `Map` prototype chain cannot collide with Map's keys
* `Map` can perform better when doing frequent addition and removals of key pairs

```javascript
// Old way to avoid prototype collisions
var container = Object.create(null);
container["key"] = 123;
container["foo"] = "bar";

// With Map
const map = new Map([["key", 123],["foo","bar"]]);
```

Note: In order to use Objects like Maps in the past without worrying about prototype collisions, it was possible to do Object.create(null), making a clean object with a `null` prototype.

---

## Creating and Using a `Map`

* Created with `new Map()`
  * An iterable giving `[key, value]` pairs can be passed to the function constructor
* Set values with `.set(key, value)`
* Get values with `.get(key)`
* Remove values with `.delete(key)`
* Check if a key exists with `.has(key)`
* For a full list, refer to [MDN - Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

---

## Basic Use of `Map`

```javascript
const m = new Map();
const m2 = new Map([[1, "one"], [2, "two"], [NaN, "Not a number"]]);
m.set("hello", "World");
console.log(m.get("hello"));
if (m2.has(NaN)) {
    m2.delete(NaN);
}
for (const [k, v] of m2) {
    console.log(`${k} - ${v}`);
}
console.log([...m2]);
```

---

## Iterating Through a `Map`

* Besides `for...of`, it is possible to use
  * `.forEach(callback[, thisArg])`
  * `.entries()` (returns an `Iterator` object with `[key, value]` pairs)
  * `.values()` (returns an `Iterator` object with the values)
  * `.keys()` (returns an `Iterator` object with the keys)
* `Map` is iterated through in insertion order

```javascript
const m = new Map([[1, "one"], [2, "two"], [NaN, "Not a number"]]);
m.forEach((v, k) => console.log(`${k} - ${v}`));
const i1 = m.entries();
console.log(i1.next().value);
const i2 = m.values();
console.log(i2.next().value);
```

---

## `Set`

* `Set` is a container of unique values
  * Values can be primitives or Object references
* `Set` uses strict equality (`===`) to check for uniqueness
  * However, `NaN` may still be used as a unique item despite `NaN !== NaN`
* Unlike Arrays, `Set` has `.size` containing the actual amount of items in the `Set`

---

## Creating and Using a `Set`

* Created with `new Set()`
  * An iterable object may be passed in to populate the `Set`
* Add an item with `.add(value)`
* Check for the presence of an item with `.has(value)`
* Remove and item with `.delete(value)`
* For a full list, refer to [MDN - Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

```javascript
const s = new Set();
const s2 = new Set([1, 2, 1, NaN, "2"]);
s.add(123);
if (s2.has(NaN)) {
    s2.delete(NaN);
}
for (const o of s2) {
    console.log(o);
}
console.log([...s2]);
```

---

## Iterating Through a `Set`

* Besides `for...of`, it is possible to use
  * `.forEach(callback[, thisArg])`
  * `.values()` (returns an `Iterator` object containing the values in the Set)
  * `Set` also has `.entries()` and `.keys()`, but they provide little added value since the key of an item in a `Set` is equal to its value
* `Set` is iterated through in insertion order

```javascript
const s = new Set([1, "1", NaN, "NaN"]);
s.forEach(v => console.log(`Value: ${v}`));
const it = s.values();
console.log(it.next().value);
```

---

## `WeakMap`

* JS has a special type of `Map` called `WeakMap`
* Like `Map`, it holds key-value pairs
* Unlike `Map`, keys **must** be object references
* Unlike other object references, the keys in a `WeakMap` are *weak references*
  * This means that they do not prevent garbage collection of the object
* `WeakMap` has limited functionality:
  * `.delete(key)`
  * `.get(key)`
  * `.has(key)`
  * `.set(key, value)`
* `WeakMap` is **not** iterable

Note: A WeakMap can be used to create privacy inside an object. [Reference](https://www.sitepen.com/blog/2015/03/19/legitimate-memory-efficient-privacy-with-es6-weakmaps/)

---

## `WeakSet`

* Similarly to `WeakMap`, JS has a `WeakSet` able to hold a unique collection of weak object references
* `WeakSet` can only contain object references
* `WeakSet` has limited functionality:
  * `.add(value)`
  * `.delete(value)`
  * `.has(value)`
* `WeakSet` is **not** iterable

---

## Appendix: Making an Object Iterable

* Add a key `Symbol.iterator` containing a generator function that yields your data
  * This implies knowledge of how to write generators

```javascript
const myIterable = {
    arrData: [ 1, 2, 3, 4 ],
    objData: { a: 5, b: 6, c: 7 },
    [Symbol.iterator]: function* () {
        yield* this.arrData;
        yield* Object.entries(this.objData);
    }
};
for (const o of myIterable) {
    console.log(o);
}
```
