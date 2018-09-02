# ES6 Iterable

---

## Overview
- Iterable
- Iterator 
- Operations that Uses Iteration
- Summary

---

# Iterable

---

## Iterable
- Anything that has a method whose key is Symbol.iterator and the method returns an iterator
- Built-in Iterable Data Structures
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
iterator is a static property on Symbol class. We can use it to access or modify internal language behavior, in this case, iteration. 
Why object is not iterable? If object is iterable, you are iterating over properties. But iteration is for iterating over data. If your object is designed to hold data, then you should use Map.

---

# Iterator

---

## Iterator
- Symbol.iterator is an iterator factory
- What is an iterator?
    - A pointer that traverses data in a container and returns one element at a time
    - It is an object that has a next() method.
- Semantics of next() method
    - next() returns an object with two properties `done` and `value`
    - `done` is a boolean indicating whether iteration is finished
    - `value` can be any type and it's the current value of the iteration. If `done`, `value` is usually ommited.

note:
When Symbol.iterator is invoked, it returns an iterator. 

---

## Iterator
### Built-in Iterable
- Let's illustrate an iterator through an array:
```javascript
const arr = [1, 2, 3];
const iterator = arr[Symbol.iterator]();
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
```

---

## Iterator
### Custom Iterable
- We can make things iterable by correctly defining Symbol.iterator property
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
const myIterable = new MyIterable('hi!');
const myIterator = myIterable[Symbol.iterator]();
console.log(myIterator.next());
console.log(myIterator.next());
console.log(myIterator.next());
console.log(myIterator.next());
```

note:
Similary we can modify iteration behavior by changing the value of its Symbol.iterator

---

# Operations that Uses Iteration 

note: 
Iteration is nice, but accessing the iterator through the iterator symbol and then access the values by calling .next is pretty cubersome.
Luckily ES6 introduced many language constructs that process iterables seamlessly, which makes this feature so much more powerful.

---

## Operations that Uses Iteration 
### Destructuring, Spread, Array.from
- Destructure an iterable and assign values to elements in an array
```javascript
const [a, b] = 'hi';
console.log(`a: ${a}, b: ${b}`);
```
- Spread an iterable into an array
```javascript
const str = '123';
const arrExpanded = [0, ...arr, 4];
console.log(`Expanded array: ${arrExpanded}`);
```
- Array.from() converts iterable to an array
```javascript
const arr = Array.from('hi!');
console.log(`Array: ${arr}`);
```

---

## Operations that Uses Iteration
### for-of
- `for-of` loops through an iterable
```javascript
const str = '123'
for (const i of str) {
    console.log(i);
}
```

---

### for loop vs. forEach() vs. for-of
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
- for-of: combines two advantages
```javascript
let sum = 0;
for (const num of numbers) {
	if (sum + num > 10)
		break;
	sum += num;
}
```

note:
Array is iterable but we already have regular for loop and forEach.
So why should we use for-of to iterate through an array?

---

## Operations that Uses Iteration
### Map and Set
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
- Map and Set are iterables themselves. You can clone a map/set using it's constructor.

---

## Operations that Uses Iteration 
### yield*
