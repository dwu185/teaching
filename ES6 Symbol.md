# ES6 Symbol

---

## Overview
- New primitive type
- Object Operations and Symbols
- Use Cases
- Symbol API

---

## Symbol
### A New Primitive Type
- Symbol is a factory function that returns a unique symbol value
    - Do not use `new`, it will throw TypeError
- Syntax: Symbol([description])
    - Description is optional. It's printed when printing the symbol
    ```javascript
    console.log(Symbol('I am a symbol value!'));
    console.log(Symbol('b') === Symbol('b'));
    ```
- Symbol is a new primitive type
```javascript
const a = Symbol('a'); 
console.log(typeof a);
```

---

## Object Operations and Symbols
### Operations that are aware of symbols
- Reflect.ownKeys()
- Object.getOwnPropertySymbols()
- Use [] to access symbol properties
- Object.assign(target, source1, source2...)
### Operations that ignore symbols
- Object.keys()
- Object.getOwnPropertyNames()
- for-in
- JSON.stringify(obj)

---

## Use Case
### Symbol as Property Names
- Symbol can be used as unique identifier for class and object literal
- It is make possible by the computed property key feature
```javascript
const a = Symbol('a'); 
const obj = {[a]: 'a'}; // expression in []
console.log(obj);
class Test {
    [Symbol('method1')]() { console.log("Method 1"); }
}
console.log(Test);
```
- No property/method name clashing in the inheritance chain
- Useful for meta-programming operations: never clash with normal properties

## Use Case
### No Name Clashing in Inheritance Chain
```javascript
class Test {
    [Symbol('method1')]() { console.log("Test1: Method 1"); }
}
class Test2 extends Test {
    [Symbol('method1')]() { console.log("Test2: Method 1"); }
}
const t1 = new Test();
const t2 = new Test2();
t1.
```

---

## Symbol APIs
To create symbols available across files and even across realms (each of which has its own global scope), use the methods Symbol.for() and Symbol.keyFor() to set and retrieve symbols from the global symbol registry.
Symbol.for(key)
Searches for existing symbols with the given key and returns it if found. Otherwise a new symbol gets created in the global symbol registry with this key.
Symbol.keyFor(sym)
Retrieves a shared symbol key from the global symbol registry for the given symbol.

## More on Symbol
### JavaScript Built-in Symbols
- Symbol.iterator: A symbol accessing default iterator for an object
- Symbol.hasInstance: A symbol accessing a method which determining if a constructor object recognizes an object as its instance. Used by instanceof.
