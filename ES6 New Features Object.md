# ES6 New Features: Object

---

## Overview
- Object Literals
    - Property Shorthand
    - Method Shorthand
    - Computed Property Name
- New Object Class Methods 

---
## Object Literals
- Use property shorthand when the property name is the same as name of the variable storing value
```javascript
const a = 1, b = 2;
const obj = {a, b};
console.log(obj);
```
- Use method shorthand when you are define a method in an object literal
```javascript
const util = {
    add (a, b) { return a + b; },
    multiply (a, b) { return a * b; }
};
```
- Use computed property name if it need to be determined at runtime or if you use symbol as property key
```javascript
const obj = {
    [Symbol('test')]: 'test',
    ['su'+'m'](a, b) { return a + b; }
};
console.log(obj);
```

---

## New Object Class Methods 
### Object.assign
- Object.assign(target, source1, source2, ...)
- Shallow copy all enumerable own properties of sources into the target object and returns the target object
```javascript
let a = {a: 1}, b = {b: 2}, c= {c: 3};
const merged = Object.assign(a, b, c);
console.log(merged);
console.log(a); // a is the target, and it is modified
```
- Properties with same name: later ones overwrite earlier ones
```javascript
let a = {a: 1}, b = {a: 2, b:2};
const merged = Object.assign(a, b);
console.log(merged);
```

---

## New Object Class Methods 
### Object.assign Continued
- Both enmerable own string properties and symbols will be copied
```javascript
let obj = {
    Symbol['test']: test
};
Object.assing({}, obj);
```
- Getter in the source will get invoked, and the returned value becomes the value for the property
```javascript
const obj = {
    get name() { return 'name'; }
};
Object.assign({}, obj);
```

---

## New Object Class Methods 
### Object.getOwnPropertySymbols
- Object.getOwnPropertySymbols(obj)
- Returns an array of an object's own (not inherited) symbols 
```javascript
const obj = {
    a: 1,
    [Symbol('b')]: 2,
    [Symbol('c')]: 3
};
Object.getOwnPropertySymbols(obj);
```

---

## New Object Class Methods 
### Object.is
- Object.is(value1, value2);
- Returns a boolean indicating whether two inputs have the same value
- Object.is vs. ===
```javascript
console.log(Object.is(NaN, NaN));
console.log(NaN === NaN);
```
```javascript
console.log(Object.is(+0, -0));
console.log(+0 === -0);
```

---

## New Object Class Methods 
### Object.setPrototypeOf
- Object.setPrototypeOf(obj, prototype)
- Sets the prototype of the object then returns it
- Warning: setPrototypeOf has negative performance impact. The best way to set prototype is at object creation time using Object.create
left column
```javascript
const proto = {
    print() {
        console.log('proto print');
    }
};
const obj = {};
Object.setPrototypeOf(obj, proto);
obj.print();
```
right column
```javascript
const proto = {
    print() {
        console.log('proto print');
    }
};
const obj = Object.create(proto);
obj.print();
```

note:
The effects on performance of altering inheritance are subtle and far-flung, and are not limited to simply the time spent in the Object.setPrototypeOf(...) statement, but may extend to any code that has access to any object whose [[Prototype]] has been altered. 

