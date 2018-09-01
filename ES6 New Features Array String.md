# ES6 New Features: Array and String

===

# New Features: Array

---

## Overview
### Array
- Array is iterable
    - `...` spread
- New Array Class Methods
- New Array Instance Methods

---

## Array is iterable
### `...` spread
- `...` can easily expand an iterable into an array
- Shallow copy an array
```javascript
const arr = [1, 2, 3]; 
const arrCopy = [...arr];
arrCopy[0] = 'altered';
console.log(`Copy=${arrCopy} Original=${arr}`);
```
- Add elements to an array
```javascript
const arr1 = [1, 2, 3], arr2 = [4, 5, 6]; 
const all = [0, ...arr1, ...arr2];
console.log(all);
```
- Use it instead of Array.prototype.concat
```javascript
const arr1 = [1, 2, 3], arr2 = [4, 5, 6]; 
const all = [0].concat(arr1, arr2);
console.log(all);
```

---

## New Array Class Methods
### Array.from
- Array.from(arrayLike[, mapFn[, thisArg]])
- Shallow-copy an array-like or iterable object and return a new array
left column
```javascript
//array-like
function f() {
    console.log(arguments instanceof Array);
    console.log(Array.from(arguments) instanceof Array);
}
f(1,2,3);
```
right column
```javascript
//iterable
const numbers = new Set([1,2,3]);
Array.from(numbers);
```
- Optional mapFn is executed on each element
```javascript
//iterable
const numbers = new Set([1,2,3]);
Array.from(numbers, i => i*i);
```

note:
array-like: have length and indexed elements. Ex: arguments and DOM operations like document.getElementsByClassName()

---

## New Array Class Methods
### Array.of
- Array.of(element0[, element1[, ...[, elementN]]])
- Return an array containing all input elements
- Array.of replaces Array constructor
```javascript
// Number of elements is greater than 1
console.log(Array.of(1,2,3));
console.log(new Array(1,2,3));
```
```javascript
// One element and it's a number
console.log(Array.of(5));
console.log(new Array(5));
```
note:
- If Array constructor is invoked with only one parameter which is a number, it returns an array with 5 empty slots

---

## New Array Instance Methods
### Array.prototype.find and findIndex
- arr.find(predicate[, thisArg])
    - Return the first element in the array for which the predicate callback returns true. Otherwise, return undefined
- arr.findIndex(predicate[, thisArg])
    - Return the index of the first element that satisfies the predicate. Otherwise, return -1.
```javascript
const arr = [1, 4, 11, 12];
const elem = arr.find(i => i > 10);
const index = arr.findIndex(i => i > 10);
console.log(`Found element ${elem} at index ${index}`);
```
- Signature of predicate callback: function(element[, index[, array]])

note:    
findIndex can be used to find index of NaN use Object.is in predicate
indexOf cannot find NaN, it uses ===

---

## New Array Instance Methods
### Array.prototype.fill
- arr.fill(value[, start[, end]])
- From specified start index to end index (exclude end), fill array with given value.
- start and end are optional with start default to 0 and end default to arr.length
```javascript
const arr = [1, 2, 3, 4, 5];
arr.fill('f', 1, 3);
```

---

## New Array Instance Methods
### Array.prototype.copyWithin
- arr.copyWithin(target[, start[, end]])
- Shallow copy elements from start index to end index (exclude end) and paste then on the same array starting at the target index
- Array size will not be modified
- start and end are optional with start default to 0 and end default to arr.length
```javascript
const arr = [1, 2, 3, 4, 5, 6];
arr.copyWithin(3, 0, 3);
```

---

## New Array Instance Methods
### Array.prototype.keys, values, entries
- Array.prototype.keys, values, entries each returns an iterable instead of an array
    - Can convert result to an array using array.from or spread `...`
```javascript
const arr = ['a', 'b', 'c'];
const keys = [...arr.keys()];
const vals = [...arr.values()];
console.log(`Keys: [${keys}] Values: [${vals}]`);
```
```javascript
const arr = ['a', 'b', 'c'];
for (const [key, val] of arr.entries()) {
    console.log(`Key: ${key} Value: ${val}`);
}
```

===

# New Features: String

---

## Overview
### String
- Template Literal (See Previous)
- String is Iterable
    - `for-of` loop
    - `...` spread
- New String Instance Methods

---

## String is Iterable
### `for-of` loop
```javascript
const str = 'abc';
for (const l of str) {
    console.log(l);
}
```
### `...` spread
- `...` can expand an iterable into an array
```javascript
console.log([...'abc']);
```

---

## New String Instance Methods
### String.prototype.startsWith
- str.startsWith(searchString[, position])
    - Whether a string starts with the input string, return a boolean
- str.endsWith(searchString[, position])
    - Whether a string ends with the input string, return a boolean
- str.includes(searchString[, position])
    - Whether a string contains the input string, return a boolean
- Optional parameter position specifies where to start the search
```javascript
const str = "Hello World!";
console.log(`startsWith 'Hello': ${str.startsWith('Hello')}`);
console.log(`endsWith 'World!': ${str.endsWith('World!')}`);
console.log(`includes 'llo Wor': ${str.includes('llo Wor')}`);
//specify start position
console.log(`startsWith 'Hello': ${str.startsWith('Hello', 1)}`);
```

---

## New String Instance Methods
### String.prototype.repeat
- str.repeat(count);
- Concatenate `count` copies of `str` and return the new string
```javascript
'a'.repeat(5);
```



