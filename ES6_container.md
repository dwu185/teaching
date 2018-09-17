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

===

# Background

---

## Background: The Old Containers

* JavaScript used to have two categories of containers
  * Arrays - Typically used for a collection of similar items
  * Objects - Allowing key-value matching using the object's properties
* Both are regular objects under the hood, with keys being `string`
  * Or `Symbol` since ES6
* ES6 introduced traditional Data Structures to contain data

---

## Background: The Old Iteration Techniques

* Iterating through arrays 
  - `Array` methods such as `map`, `forEach`, `reduce`, `filter`
  - Tranditional loops `for`, `while`, `do...while`
* Iterating through objects
  - Object methods such as `Object.keys()` or `Object.getOwnPropertyNames()`
  - `for...in` loop
* ES6 introduced the concept of **Iterable Objects**, which can be more easily traversed than traditional object

===

# `for...of`

---

## `for...of`

* ES6 introduces `for...of`
* It allows iterating through the values of any **iterable**
* Arrays and strings are both iterable
* ES6 `Map` and `Set` are also iterable

```javascript
const arr = [1, 2, 3, 4, 5];
for (const n of arr) {
    console.log(n);
}

const str = "Hello World";
for (const l of str) {
    console.log(l);
}
```

===

# `Map`

---

## `Map`

* `Map` is a container holding key-value pairs
* Keys can be any type (both objects and primitives)
  * `Map` is able to handle `NaN` as a unique value despite `NaN !==NaN`
* `Map` is iterable
* `typeof` a map is 'object'

---

## Differences Between `Map` and `Object`

* `Map` has some useful APIs such as `.size()`, `.clear()` etc.
* Properties of `Map` prototype chain cannot collide with data stored in a map
  * It is not the case for objects: interested side effects when adding/deleting a key/value pair

```javascript
// Old way to avoid prototype collisions
var container = Object.create(null);
container["key"] = 123;
container["foo"] = "bar";

// With Map
const map = new Map([["key", 123],["foo","bar"]]);
```

* `Map` can perform better when doing frequent additions and removals of data
* Order of data insertion is preserved
* Plain objects are not iterable

note: 
In order to use Objects like Maps in the past without worrying about prototype collisions, it was possible to do Object.create(null), making a clean object with a `null` prototype.

---

## Creating and Using a `Map`

* Created with `new Map()` or `new Map(iterable)`
  * An iterable of `[key, value]` pairs can be passed to the constructor
* Store a key/value pair with `.set(key, value)`
* Get value of a given key with `.get(key)`
* Remove a key/value pair with `.delete(key)`
* Check if a key exists with `.has(key)`
* For a full list, refer to [MDN - Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

---

## Basic Use of `Map`

```javascript
const m = new Map();
m.set("hello", "World");
console.log(m.get("hello"));
```

```javascript
const m = new Map([[1, "one"], [2, "two"], [NaN, "Not a number"]]);
if (m.has(NaN)) {
    m.delete(NaN);
}
console.log(m.has(NaN));
```

---

## Iterating Through a `Map`

* Any JavaScript features that can handle iterables can handle maps
* We can use `for...of` or spread operator `...`

```javascript
const m = new Map([[1, "one"], [2, "two"]]);
for (const [k, v] of m) { //destructuring
    console.log(`${k} - ${v}`);
}
console.log([...m]);
```

---

## Iterating Through a `Map`

* `Map` also have some built-in APIs
  * `.forEach(callback[, thisArg])`
  * `.entries()` (returns an iterable with `[key, value]` pairs)
  * `.values()` (returns an iterable with the values)
  * `.keys()` (returns an iterable with the keys)

```javascript
const m = new Map([[1, "one"], [2, "two"]]);
m.forEach((v, k) => {
  //callback: value is the first parameter
  console.log(`${k}: ${v}`);
});
console.log([...m.keys()]);
console.log([...m.values()]);
```
* `Map` is iterated through in insertion order

===

# `Set`

---

## `Set`

* `Set` is a container of unique values
* Values can be any type (both objects and primitives)
  * `NaN` is treated as one unique value despite `NaN !== NaN`
  * 0, -0 and +0 are also treated as one unique value
* `Set` is iterable
* `typeof` a set is 'object'

```javascript
console.log(new Set([NaN, NaN, NaN]));
console.log(new Set([0, -0, +0]));
```

---

## Creating and Using a `Set`

* Created with `new Set()` or `new Set(iterable)`
  * An iterable object may be passed in to populate the `Set`
* Add an item with `.add(value)`
* Check for the presence of an item with `.has(value)`
* Remove and item with `.delete(value)`
* For a full list, refer to [MDN - Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

```javascript
const s = new Set();
s.add(123);
console.log(s);
```

```javascript
const s2 = new Set([1, 2, 1, NaN, "2"]);
if (s2.has(NaN)) {
    s2.delete(NaN);
}
console.log(s2);
```

---

## Iterating Through a `Set`

* Any JavaScript features that can handle iterables can handle maps
* We can use `for...of` or spread operator `...`

```javascript
const s = new Set([1, 2, 1, NaN, "2"]);
for (const o of s) {
    console.log(o);
}
console.log([...s]);
```
---

## Iterating Through a `Set`

* `Set` also have some built-in APIs
  * `.forEach(callback[, thisArg])`
  * `.values()` (returns an iterable containing the values in the Set)
  * `.entries()` and `.keys()` provide little value since the key of an item in a `Set` is equal to its value

```javascript
const s = new Set([1, "1", NaN, "NaN"]);
s.forEach(v => console.log(`Value: ${v}`));
console.log([...s.values()]);
```
* `Set` is iterated through in insertion order

===

# `WeakMap` and `WeakSet`

---

## `WeakMap`

* ES6 also introduced a special type of `Map` called `WeakMap`
* Like `Map`, it holds key-value pairs
* Unlike `Map`, keys **must** be object references
* Unlike other object references, the keys in a `WeakMap` are *weak references*
  * They do not prevent garbage collection if there are no other references to the values
* `WeakMap` has limited functionality:
  * `.delete(key)`
  * `.get(key)`
  * `.has(key)`
  * `.set(key, value)`
* `WeakMap` is **not** iterable
* WeakMap can be used as a mechanism to achieve privacy

note: 
A WeakMap can be used to create privacy inside an object. [Reference](https://www.sitepen.com/blog/2015/03/19/legitimate-memory-efficient-privacy-with-es6-weakmaps/)

---

## `WeakSet`

* Similarly to `WeakMap`, JavaScript has a `WeakSet` able to hold a unique collection of weak object references
* `WeakSet` can only contain object references
* `WeakSet` has limited functionality:
  * `.add(value)`
  * `.delete(value)`
  * `.has(value)`
* `WeakSet` is **not** iterable

===

# Exercise

---

## Exercise
- Save object's own iterable properties into a map
```javascript
const obj = {a: 'a', b: 'b', c: 'c'};
function getMapFromObj(obj) {
  //TODO
}
console.log(getMapFromObj(obj));
```
- Clean up the code below by replacing array with set
```javascript
function bulkAdd(set, values) {
  for (const val of values) {
    if (set.indexOf(val) === -1) {
      set.push(val);
    }
  }
  return set;
}
function bulkRemove(set, values) {
  for (const val of values) {
    const index = set.indexOf(val);
    if (index !== -1) {
      set.splice(index, 1);
    }
  }
  return set;
}
console.log(bulkAdd([], [1,1,2,2,3]));
console.log(bulkRemove([1, 2, 3], [1, 4, 3]));
```

note:
```javascript
const obj = {k1: 'v1', k1: 'v2', k3: 'v3'};
function getMapFromObj(obj) {
  const map = new Map();
  for (const prop in obj) {
    if (obj.hasOwnProperty(prop)) {
      map.set(prop, obj[prop]);
    }
  }
  return map;
}
console.log(getMapFromObj(obj));
```
```javascript
function bulkAdd(set, values) {
  for (const val of values) {
    set.add(val);
  }
  return set;
}
function bulkRemove(set, values) {
  for (const val of values) {
    const index = set.indexOf(val);
    if (index !== -1) {
      set.splice(index, 1);
    }
  }
  return set;
}
console.log(bulkAdd([], [1,1,2,2,3]));
console.log(bulkRemove([1, 2, 3], [1, 4, 3]));
```
