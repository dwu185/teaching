# ES6 Rest and Spread

---

## Overview
- Rest Parameters `...`
- Spread Operator `...`

note:
We will cover rest and spread together because they have the same notation `...`. Hopefully by covering them side by side, we can get a clearer understanding of both.

---

## Rest Parameters
### Use Case: Function Definition
- Rest must be the last one in the argument list
- It gathers the "rest" of the arguments in an array
- We can replace ES5 `arguments` with Rest
```javascript
//ES5 using arguments
function test (first) {
    var rest = [].slice.call(arguments, 1);
    console.log(`first: ${first}, rest: ${rest}`);
}
test('a', 'b', 'c');
```
```javascript
//ES6 using rest
function test (first, ...rest) {
    console.log(`first: ${first}, rest: ${rest}`);
}
test('a', 'b', 'c');
```
- Another benefit of rest: more self-documenting 

note:
arguments is array-like object instead of a real array. We had to create an array by using slice and call. 
Also arrow functions don't have 'arguments', so if used within function body, it will use the 'arguments' from the outer non-arrow function.

---

## Rest Parameters 
### Destructuring
- Function definition
```javascript
function test ([first, ...rest]) {
    console.log(`First: ${first}, Rest: [${rest}]`);
}
console.log(test([1, 2, 3]));
```
- Assignment
```javascript
const [first, ...rest] = [1, 2, 3];
console.log(`First: ${first}, Rest: [${rest}]`);
```

note:
ES9: can use rest to destructure object literals

---

## Spread Operator
### Use Case: Function Call
- Rest parameters is used in function defintion. Spread operator can be used in function call.
- It spreads an iterable into parameters of a function
```javascript
const arr = [3, 4, 2, 5];
console.log(Math.max(...arr));
// equivalent to: Math.max(3, 4, 2, 5);
```
- Unlike rest, spread does not need to be the last
```javascript
const arr1 = [3, 4], arr2 = [2, 5]
console.log(Math.max(...arr1, 0, ...arr2, 10));
```

note: 
Array is an iterable.
Built-in function Math.max is a good example. It takes in a variable number of input and return the max number. If we have an array of numbers, we can use the spread operator to invoke Math.max.

---

## Spread Operator
### Use Case: Array Assignment
- Spread can easily expand an iterable into an array
- Shallow copy an array
```javascript
const arr = [1, 2, 3]; 
const arrCopy = [...arr];
arrCopy[0] = 'altered';
console.log(`Copy=[${arrCopy}] Original=[${arr}]`);
```
- Add elements to an array
```javascript
const arr1 = [1, 2, 3], arr2 = [4, 5, 6]; 
const all = [0, ...arr1, ...arr2, 7];
console.log(all);
```
- Use it instead of Array.prototype.concat
```javascript
const arr1 = [1, 2, 3], arr2 = [4, 5, 6]; 
const all = [0].concat(arr1, arr2, 7);
console.log(all);
```

note:
ES9: spread object literal

---

## Summary
- `...` is rest 
    - Function definition: At the end of function parameters
    - Destructure Array: On the left hand side of assignment
- `...` is spread
    - Function call: spread iterable into parameters
    - Constructure Array: Can be on the right hand side of assignment 