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

---

## Class In Detail

### Class vs. Function Constructor

- Class can **only** be invoked via `new`, not via a function call
- Class can only contain methods and constructor 
    - Instance properties must be used in class methods/constructor
- Class has a cleaner way to call parent constructor and methods via `super`
- Function declarations are hoisted, class declarations are not

note:
Let's have a summary of the main differences between class and function constructor...

---

## Class In Detail

### Constituents of a Class

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

## Class in Detail

### Code Example

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

# Inheritance Overview

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
## Inheritance

### `extends`

- Employee is a subclass of Person, which inherits all functionalities of Person

```javascript
class Employee extends Person {
    //...
}
let emp = new Employee('Ken', 22);
emp instanceof Person; //true
emp instanceof Employee; //true
emp.name; //Ken
emp.print(); //Name: Ken, Age: 22
```

- `extends` can be followed by a class or a function constructor
- `extends` can also be followed by an expression
    - This is why classes are not hoisted

note:
Let's start with keyword `extends`.
Use keyword `extends` to specify the parent class. Employee is a subclass of Person,
 which inherits all functionalities of Person class including name getter and print method.
Note that the parent following `extends` keyword can be either class or function constructor.
`extends` can also be followed by an expression that evaluates to be the parent.

---

## Inheritance

### `super`

Two ways to use `super`:

- Call the parent class constructor
    - It must be called before accessing 'this' or return
- Call the parent's method

<!--DIV-LEFTCOLUMN-->

```javascript
class Employee extends Person {
    constructor(name, age, occupation) {
        //1. call parent class constructor
        super(name, age);
        this._occupation = occupation;
    }
    *[Symbol.iterator]() {
        //2. call parent's method
        yield* super[Symbol.iterator]();
        yield this._occupation;
    }
}
```

<!--DIV-RIGHTCOLUMN-->

```javascript
let emp = new Employee('Ken', 22, 'Teacher');
for (let i of emp) console.log(i);
/*
Ken
22
Teacher
*/
```

note:
There are two ways to use `super`...
"Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor".
In the code example below, Employee class defines it's own constructor function
which invoke the parent's constructor. It also extends the iterator using the iterator of the parent.

===

# Private Properties in Classes?

- Naming Convention: Underscore prefix
- Closure: Use private properties inside constructor only
- Symbols as property keys
- Store private properties in WeakMaps

note:
Moving on to another important idea: privacy.
How does javascript keep data private? Here's a list of four ways, and we
will cover each in more details...

---

## Privacy

### Naming Convention

```javascript
class Person {
    constructor(name, age) {
        this._name = name;
        this._age = age;
    }
    print() {
        console.log(`Name: ${this._name}, Age: ${this._age}`);
    }
}
```

- Code is easy to read
- Properties are not private
- Subclasses of Person can **accidentally** have properties with the same names

note:
One popular mechanism to make our state more ‘private’ is using naming conventions
 like an underscore in front of the property name. It's a simple solution with clean
 looking code. The drawdacks are: first naming doesn't make a property private, it merely
 signals the intent of the author. Second of all, if the same property name is accidently
 used by the subclass, this can cause issues that will be hard to debug, especially
 if the author has no access to parent class's source code.

---

## Privacy

### Closure

```javascript
class Person {
    constructor(name, age) {
        this.print = () => { console.log(`Name: ${name}, Age: ${age}`); }
    }
}
```

- Properties are truly private
- All methods that use private data must be defined in the constructor function
- All methods that use private data are stored on each instance instead of being
stored once on Person.prototype.

note:
Another approach is taking advantange of JavaScript closure by keeping private
properties within constructor function.
On the plus side, properties are truly private.
On the downside, this approach is heavy on memory. Keeping properties in the contructor
means we need to define methods (at least those that use private properties) within
the constructor as well. These methods are stored on every instance instead of being stored
only once on Person.prototype. prototype methods are shared, intance methods are not

---

## Privacy

### Symbols as property keys

```javascript
const nameSymbol = Symbol('name');
const ageSymbol = Symbol('age');
class Person {
    constructor(name, age) {
        this[nameSymbol] = name;
        this[ageSymbol] = age;
    }
    print() {
        console.log(`Name: ${this[nameSymbol]}, Age: ${this[ageSymbol]}`);
    }
}
```

- No name clashing because by definition each symbol is unique
- Symbols help to make properties harder to access but not impossible
- APIs like Reflect.ownKeys(), Object.getOwnPropertySymbols() expose symbols

note:
Another alternative is using Symbols as property keys.
It makes properties hard to access and there will be no property name clashing between
parent class and subclasses since each Symbol is unique.
The downside is that though Symbol makes accessing private data a challenge, it is not impossible...

---

## Privacy

### Store private properties in WeakMaps

```javascript
const nameWM = new WeakMap();
const ageWM = new WeakMap();
class Person {
    constructor(name, age) {
        nameWM.set(this, name);
        ageWM.set(this, age);
    }
    print() {
        console.log(`Name: ${nameWM.get(this)}, Age: ${ageWM.get(this)}`);
    }
}
```

- Code is harder to read
- As long as weak maps are not exposed, properties are private
- Special case: malicious program overwrote WeakMap.prototype.set/get
- We use WeakMap instead of Map because WeakMap doesn't prevent garbage collection

note:
The last and final mechanism we’ll cover is using WeakMap in ES6 to achieve privacy.
If you want to learn more about WeakMap, check out the ES6 Containers ELRN video.
Here we use WeakMap as the storage for our private properties. One WeakMap for each property.
The key of the maps is an object instance and the value is the private property value of the instance.
As long as we do not expose the WeakMaps to the outside world, we are guaranteed privacy.
It is much safer than using naming or symbol. However, the code is now harder to read.
And malicious program that runs before our code than potentially replace
WeakMap.prototype.get and .set methods with functions that exposes our private data.

---
## Privacy

### In the Future!

- Three TC39 proposals focusing on adding Privacy to Classes

```javascript
class Person {
    constructor(name, age) {
        this.#name = name;
        this.#age = age;
    }
    print() {
        console.log(`Name: ${this.#name}, Age: ${this.#age}`);
    }
}
```

- Syntax: property name with hash `#` prefix
- No code outside of Person class can detect or affect the existence, name or value of its private properties
    - Including subclasses and superclasses

note:
To make properties private, just give them a name starting with #.
No code outside of Person class can detect or affect the existence, name or value of private properties.
This includes subclasses and superclasses.

===

# ES6 Class in Bloomberg

note:
Can you use ES6 classes in Bloomberg?

---

## ES6 Class in Bloomberg

### R+

{BLST /ID 6564258207119704096}

- ES6 classes are supported and encouraged in R+
- JSCore.class has performance penalties
- JSCore.class should only be used in case where hard privacy is required

note:
Robert Pamely from JavaScript Guild sent out a blast covering the best practices for classes in R+.
It is also in the R+ documentation.
I recommend reading the R+ documentation, but here are the main takeaways...

---

## ES6 Class in Bloomberg

### Rapid (Very likely change in the future)

- Rapid also support Classes if your codebase is ES6 or TypeScript
- Currently, classes are transpiled before running in SpiderMonkey (we use an older version).
- Module is introduced to Rapid, you can take full advantage of classes by sharing code across modules.
