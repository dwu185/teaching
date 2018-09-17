---
layout: bb-reveal-single-slide
title: ES6 Class
controlsFlag: true
---

# ES6 Class

===

## Overview

- ES5: Use constructor **functions** to create similar instances
- Now: Use **ES6 classes** (mainly syntactic sugar for constructor functions)

<!--DIV-LEFTCOLUMN-->
```javascript
//ES5
function Person(name, age) {
    this._name = name;
    this._age = age;
}
Person.prototype.print = function () {
    console.log('Name: ', this._name, ', Age: ', this._age);
}
const p = new Person('Bob', 42);
```

<!--DIV-RIGHTCOLUMN-->
```javascript
//ES6
class Person {
    constructor(name, age) {
        this._name = name;
        this._age = age;
    }
    print() {
        console.log(`Name: ${this._name}, Age: ${this._age}`);
    }
}
const p = new Person('Bob', 42);
```

note:
ES5: We use constructor **functions** as factories to create similar instances
Now: We can use **ES6 classes**, which mainly is syntactic sugar for constructor functions
There's an example comparing ES5 and ES6 approach. Note how ES5 uses prototype property of
JavaScript function to store instance methods while ES6 stores methods within class block.
Instantiation of a new person instance is the same for both using the `new` keyword.

===

# Class In Detail

---

## Constituents of a Class

- Constructor
    - Default constructor for base class:
    ```javascript
    constructor () {}
    ```
    - Default constructor for derived class:
    ```javascript
    constructor (...args) { super(...args); }
    ``` 
- Methods (instance and static)
- Getters and Setters (instance and static)
- Generator methods (instance and static)

note:
Here's a list of things that can live in a class body...
Everything in the list is optional including the constructor.
If constructor function is missing, default constructor is provided.
We cannot use data properties directly in the class body, we can workaround it
using static getter or use dot to store properties directly on the class.

---

## Code Example

```javascript
class Person {
    constructor(name, age) { // constructor
        this._name = name;
        this._age = age;
    }
    print() { // instance method
        console.log(`Name: ${this._name}, Age: ${this._age}`);
    }
    get name() { // getter
        return this._name;
    }
    set name(newName) { // setter
        this._name = newName;
    }
    static classification () { // static method
        return 'Mammal';
    }
    *[Symbol.iterator]() { // generator with computed name
        yield this._name;
        yield this._age;
    }
}
```

note:
Person class contains an example of each item on the list...
We use a generator here to implement an iterator (you can watch the iterator
and generator ELRN videos for more information).
Another thing worth pointing out is that a class can have computed method name by putting
 expression within square brackets.

===

# Inheritance

---

## Inheritance Overview

ES6 adds new keywords to provide an easier way to create a subclass

- `extends`
- `super`

A subclass

- can inherit from parent class
- can overwrite and extend parent class functionalities
- is an instance of parent class (`instanceof`)

ES6 Classes only support single inheritance

note:
Now let's go over another important feature of Class: Inheritance...

---

## `extends` and `super`

Use keyword `extends` to specify the parent class
Two ways to use `super`:
- Call the parent class constructor
- Call the parent's method

<!--DIV-LEFTCOLUMN-->
```javascript
class Person {
    constructor (name) {
        this._name = name;
    }
    get name() {
        return this._name;
    }
}
class Employee extends Person {
    constructor(name, occupation) {
        super(name);
        this._occupation = occupation;
    }
    get occupation() {
        return this._occupation;
    }
}
const emp = new Employee('Ben', 'Teacher');
console.log(emp instanceof Person);
console.log(emp instanceof Employee);
console.log(emp.name());
console.log(emp.occupation());
```

note:
There are two ways to use `super`...
"Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor".

===

# Exercise

---

## Exercise

ES6 allows subclassing built-in constructors. It is usually not the recommend practice. But just for this exercise, please implement the Stack class by inheriting from the Array class. 
Please implement Stack interface:
- isEmpty, pop, push, top
```javascript
const stack = new Stack();
stack.push([1,2,3,4]);
stack.pop();
stack.top();
stack.isEmpty();
```

note:
```javascript
class Stack extends Array {
    top () {
        return this[this.length - 1];
    }
    isEmpty () {
        return this.length === 0;
    }
}
const stack = new Stack();
stack.push([1,2,3,4]);
stack.pop();
stack.top();
stack.isEmpty();
```