# ES6 Symbol

---

## Overview
- Syntax
- Symbol as Object Property
- Symbol for Privacy
- Symbol across Realms 
- Symbol for Metaprogramming 

---

## Symbol
### Syntax
- Symbol is a factory function that returns a **unique** symbol value
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

note:
As you can see in the example below: when we print a symbol, the description is printed for debugging purposes only! Each symbol created using the symbol factory is unique even if they have the same description

---

## Use Case
### Symbol as Object Property
- Computed property key for object and class
```javascript
const obj = {
    [Symbol('prop')]: 'value'
};
console.log(obj);
```

note:
ES6 has a new feature called computed property key for objects and classes.
It means we can have property names calculated at run-time by putting zn expression in a pair of brackets.
This feature enables us we to use symbols as unique identifiers for objects.

---

## Use Case
### Symbol as Object Property
- Operations that are aware of symbols
    - Reflect.ownKeys()
    - Object.getOwnPropertySymbols()
    - Use [] to access symbol properties
    - Object.assign(target, source1, source2...)
```javascript
const testSym = Symbol('test');
let obj = { [testSym]: 'test', prop: true };

console.log(Reflect.ownKeys(obj));
console.log(Object.getOwnPropertySymbols(obj));
console.log(obj[testSym]);

let target = { [Symbol('test2')]: 'test2' };
Object.assign(target, obj);
console.log(Object.getOwnPropertySymbols(target));
```

note:
Since one important use case of symbols is symbols as object properties,
we'll go over some object operations that are aware of symbols with an example object which contains a symbol property and a string property

---

## Use Case
### Symbol as Object Property
- Operations that ignore symbols
    - Object.keys()
    - Object.getOwnPropertyNames()
    - for-in
    - JSON.stringify()
```javascript
const testSym = Symbol('test');
let obj = { [testSym]: 'test', prop: true };

console.log(Object.keys(obj));
console.log(Object.getOwnPropertyNames(obj));
for (const i in obj) {
    console.log(`key=${i} value=${obj[i]}`);
}
console.log(JSON.stringify(obj));
```

note:
There are also object operations that will ignore property symbols.
Here have another object which has a symbol property and string property.

---

## Use Case
### Symbol for Privacy
- Copy from class slides
- No property/method name clashing in the inheritance chain

note:
In previous slide, we showed how Symbol keys are not very discoverable in an object. Because they are hard to access, you can use symbols to define more private properties. Note that it is not true privacy: there are still a few APIs that can expose Symbols.

---

## Use Case
### Symbol Across Realms 
- Symbols created with Symbol() are local symbols
- Symbols created with Symbol.for(key) method are global symbols
    - Global Symbol Register
```javascript
let iframe = document.createElement('iframe');
iframe.src = String(window.location);
document.body.appendChild(iframe);
//Creates global symbol in current frame
const sym = Symbol.for('foo'); 
console.log(sym === iframe.contentWindow.Symbol.for('foo'));
```
- Distinguish local and global symbol with Symbol.keyFor(sym), which retrieves a the symbol key from the global symbol registry for a given symbol
```javascript
const local_sym = Symbol('local');
const global_sym = Symbol.for('global');
console.log(`Local Symbol: ${Symbol.keyFor(local_sym)}`);
console.log(`Global Symbol: ${Symbol.keyFor(global_sym)}`);
```

note: 
In previous slides, we covered how every symbol created by the symbol factory is unique, that is because symbols created this way are local symbols. Using the static method Symbol.for(key), we can also create global symbols in a register called global symbol register.
What's special about the global symbol register is that it is cross-realms. For example, a global symbol created from one iframe is the same symbol in the existing frame.
Symbol.for with input `key` searches for symbol with the given key in the global symbol register and returns it if found. Otherwise a new symbol gets created in the global symbol registry with this key.

---

## Use Case
### Symbol for Metaprogramming 
- Static properties on Symbol class give developers access to some build-in symbols representing JavaScript's internal language behaviors
- Some Examples:
    - Symbol.iterator: for...of
    - Symbol.match: String.prototype.match
    - Symbol.hasInstance: instanceof
    - etc.
```javascript
class MyString {
    static [Symbol.hasInstance](lhs) {
        return typeof lhs === 'string';
    }
}
console.log("test" instanceof MyString);
```

note: 
The fact that symbol is unique makes it a suitable tool for metaprogramming in JavaScript. 
These static symbols alsters the internals of the JavaScript engine.

