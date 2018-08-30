
# ES6 Destructuring

---

## Overview
- Destructuring is a way to extract multiple values from an array or object and assign them to target variables
- Destructuring can be especially useful in extracting data from nested source
- It can be used in locations that expect an assignment
	- Variable declarations / assignment
	- `for-of` loop head
	- Function parameters

---

## Motivation
JavaScript has syntax to store multiple values in an array or object, but there is no corresponding mechanism to extract multiple values.
```javascript
// Object literal: store multiple properties at once
const person = {
	name: 'Jane',
	id: 140,
	birthday: {year: 1991, month: 11, day: 23}
}
// Extract value: one at a time
const birthYear = person.birthday.year;
```
```javascript
// Array: store multiple values at once
const numbers = [1, 4, 7]; 
// Extract value: one at a time
const oneNumber = numbers[0];
```

---

# Variable Declarations / Assignment

---

## Variable Declarations / Assignment
### Overview
- Use destructuring for variable declaration and assignment
```javascript
const [a] = ['a'];
console.log(a);
```
- Declaration can be separated from Assigment
```javascript
let a;
[a] = ['a'];
console.log(a);
```

---

## Variable Declarations / Assignment
### Syntax
- RHS of assignment is the source of destructuring
	- Most commonly, RHS is an object or an iterable (like array)
	- We'll go through other cases
- LHS of assignment specifies the target of destructuring
	- What values we want to extract and where to store them

note:
ES6 introduces a new concept: iterable, array is an iterable
---

## Variable Declarations / Assignment
### Target: What can be on the LHS of an assignment?
- Object Destructuring Pattern specifying what properties to extract
```javascript
// Destructuring Object
const person = {
	name: 'Jane',
	id: 140,
	birthday: {year: 1991, month: 11, day: 23}
}
const {name, id} = person;
console.log(`${name} ${id}`); 
```
- Array Destructuring Pattern specifying what values to extract
```javascript
// Destructuring Array
const numbers = [1, 4, 7]; 
const [first, second] = numbers;
console.log(`${first} ${second}`);
```

note:
If you think destructuring as pattern matching, the LHS can
be either an object pattern or an array pattern.

---

## Variable Declarations / Assignment
### Source: What can be on the RHS of an assignment?
- Objects (ex: person) and Iterables (ex: numbers)
	- Array is an iterable
- Coercion
	- If LHS is object pattern, coerce RHS to object
	- If LHS is array pattern, coerce RHS to iterable
```javascript
//string literal coerced to string object
const { length } = 'abc';
console.log(length);
```
```javascript
//string literal coerced to iterable
const [first, second] = 'abc';
console.log(`${first} ${second}`);
```

---

## Array Destructuring
### More Features 1
- Nested Arrays 
```javascript
const [a, [b, c]] = ['a', ['b', 'c']];
console.log(`${a} ${b} ${c}`);
```
- Default Values
```javascript
let a, b, c;
[a = 'a', [b, c] = ['b', 'c']] = [];
console.log(`${a} ${b} ${c}`);
[a='a', [b='b', c='c']=[]] = [];
console.log(`${a} ${b} ${c}`);
```
note:
on the LHS, each element in the array is either a variable or another
destructuring pattern

---

## Array Destructuring
### More Features 2
- Ignore some slots with empty `,`
```javascript
const [a, , c] = ['a', 'b', 'c'];
console.log(`${a} ${c}`);
```
- Rest operator `...` 
```javascript
const [a, ...others] = ['a', 'b', 'c'];
console.log(`${others}`);
```

note:
rest operator always need to be at the end

---

## Object Destructuring
### More Features 1
- Assign to variables with same names 
```javascript
const {a, b} = {a: 'a', b: 'b'};
console.log(`${a} ${b}`);
```
- Assign to variables with different names
```javascript
const {a: letterA, b: letterB} = {a: 'a', b: 'b'};
console.log(`${letterA} ${letterB}`);
```
note:
left side of the `:` indicates properties we want extract.
Right side of the `:` is for assignment, it can be either a variable (like example above) or it can be another pattern to match (we'll show an example in the next slide.)

---

## Object Destructuring
### More Features 2
- Nested Object
```javascript
const {a, more: {b, c}} = { a: 'a', more: {b: 'b', c: 'c'} };
console.log(`${a} ${b} ${c}`);
```
- Default Values
```javascript
let a, b, c;
// Note: cannot start a statement with {
({a = 'a', more: {b, c} = {b: 'b', c: 'c'}} = {});
console.log(`${a} ${b} ${c}`);
({a = 'a', more: {b = 'b', c = 'c'} = {} } = {});
console.log(`${a} ${b} ${c}`);
```

note:
Cannot start destructuring statement with `{`, we have declaration separated from assignment, so wrapped statement in ()

---

# `for-of` Loop Head

---

## `for-of` Loop Head
### Examples
- `for-of` loop can iterate through iterables
- We can use destructuring in the loop head of `for-of` loop
```javascript
//Map is an iterable
const map = new Map([
	['key1', 'val1'],
	['key2', 'val2']
]);
for (const [key, val] of map) {
	console.log(`${key}: ${val}`);
}
```
note:
We will cover for-of loop and iterables in more details in a separate video
What need to know now: for-of loop iterates through iterables

---

# Function Parameters

---

## Function Parameters
### Basic Examples
```javascript
// Object destructuring
function letters ({a, b, c}) {
	console.log(`${a} ${b} ${c}`);
}
letters({a: 'a', b: 'b', c: 'c'});
```
```javascript
// Array destructuring
function letters ([a, b, c]) {
	console.log(`${a} ${b} ${c}`);
}
letters(['a', 'b', 'c']);
```

---

## Function Parameters
### Rest Operator `...`
- Parameter with `...` in front of it receives the rest of the parameters in an array
- Parameter with `...` must be the last parameter
- Use `...` to replace `arguments` object
```javascript
// ES6
function letters(a, ...more) {
	console.log(more);
}
letters('a', 'b', 'c');
```
```javascript
// ES5
function letters() {
	// arguments is array-like
	const more = Array.prototype.slice.call(arguments, 1);
	console.log(more);
}
letters('a', 'b', 'c');
```

---

## Function Parameters
### Default Values
- Examples for Array and Object
```javascript
// Object destructuring
function letters ({a='a', b='b', c='c'}={}) {
	console.log(`${a} ${b} ${c}`);
}
letters();
```
```javascript
// Array destructuring
function letters ([a='a', b='b', c='c']=[]) {
	console.log(`${a} ${b} ${c}`);
}
letters();
```

---

## More on Default Values
- Default value is used only if the value is otherwise `undefined`
- Default value can be any expression and is computed on demand
- Default values can refer to other variables in the same pattern
	- Therefore, order matters
```javascript
const [a, b=a] = ['a'];
console.log(`${a} ${b}`);
```
```javascript
const {a, b=a} = {a: 'a'};
console.log(`${a} ${b}`);
```
```javascript
function letters(a, b=a) {
	console.log(`${a} ${b}`);
}
letters('a');
```
