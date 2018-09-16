
Symbol:
ES6 has a global resource for creating symbols: the symbol registry. The symbol registry provides us with a one-to-one relationship between strings and symbols. The registry returns symbols using Symbol.for( key ).
Symbol.for( key1 ) === Symbol.for( key2 ) whenever key1 === key2. This correspondance works even across service workers and iframes.
- Private
Even though Symbols do not make attributes private, they can be used as a notation for private properties. You can use symbols to separate the enumeration of public and private properties, and the notation also makes it clear.
```javascript
const Square = (function() {
 
    const _width = Symbol('width');
 
    class Square {
        constructor( width0 ) {
            this[_width] = width0;
        }
        getWidth() {
            return this[_width];
        }
    }
 
    return Square;  
 
} )();
```
- Avoid name clashing
- Well known symbols
- Lab
    - Write an Employee type. Each employee instance has an age property and a name property. The age of an employee should not be public. There need to be an isAdult method which returns true if employee is 21 or older.
    ```javascript
    const Employee = (function() {
        const _age = Symbol('age');
        const _name = Symbol('name');
        function Employee(age, name) {
            this[_age] = age;
            this[_name] = name;
        }
        Employee.prototype.isAdult = function () {
            return this[_age] >= 21;
        }
        return Employee;
    })();
    let e = new Employee(21, 'Ben');
    e.isAdult(); //true
    ```
    ```javascript
    const Employee = (function() {
        const _age = Symbol('age');
        const _name = Symbol('name');
        class Employee {
            constructor (age, name) {
                this[_age] = age;
                this[_name] = name;                
            }
            isAdult () {
                return this[_age] >= 21;
            }
        }
        return Employee;
    })();
    let e = new Employee(21, 'Ben');
    e.isAdult(); //true
    ```
 Iterable:
 It is worth for you to learn about iterators, especially if you are a fan of lazy evaluation, or you want to be able to describe infinite sequences. Understanding iterators also helps you understand generators, promises, sets, and maps better.

 


Labs
- Generator
    - yield* print binary tree
    - data consumer: wrapper generator is ready to consumer input right away
        /**
        * Returns a function that, when called,
        * returns a generator object that is immediately
        * ready for input via `next()`
        */
        function coroutine(generatorFunction) {
            return function (...args) {
                const generatorObject = generatorFunction(...args);
                generatorObject.next();
                return generatorObject;
            };
        }
        const wrapped = coroutine(function* () {
            console.log(`First input: ${yield}`);
            return 'DONE';
        });
        const normal = function* () {
            console.log(`First input: ${yield}`);
            return 'DONE';
        };
        > wrapped().next('hello!')
        First input: hello!
