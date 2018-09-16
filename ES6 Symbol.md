---
layout: bb-reveal-single-slide
title: ES6 Symbol
controlsFlag: true
---

# ES6 Symbol

---

## Overview
- Syntax
- Use Cases
    - Symbol as Object Property
    - Symbol for Privacy
    - Symbol across Realms
    - Symbol for Metaprogramming

===

# Symbol Syntax

---

## Symbol Syntax
- Symbol is a factory function that returns a **unique** symbol value
    - Do not use `new`, it will throw `TypeError`
- Syntax: `Symbol([description])`
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

===

# Symbol Use Cases

---

## Symbol as Object Property
- ES6 addes a new feature called computed property key for objects and classes
- Enables property names to be calculated at run-time
- Enables us to use symbols as unique property names
    - One benefit is that therewill be no name clashes

```javascript
const obj = {
    [Symbol('prop')]: 'value'
};
console.log(obj);
```

---

## Symbol as Object Property
### Operations That Are Aware of Symbols
- Use `[]` to access symbol properties
- `Object.getOwnPropertySymbols()`
- `Reflect.ownKeys()`

```javascript
const testSym = Symbol('test');
let obj = { [testSym]: 'test', prop: true };

console.log(obj[testSym]);
console.log(Reflect.ownKeys(obj));
console.log(Object.getOwnPropertySymbols(obj));
```
- `Object.assign(target, source1, source2...)`

```javascript
let target = { [Symbol('test')]: 'test', prop: true };
let source = { [Symbol('test2')]: 'test2' };
Object.assign(target, source);
console.log(target);
```

---

## Symbol as Object Property
### Operations That Ignore Symbols
- `Object.keys()`: returns an array of own, enumerable string properties
- `Object.getOwnPropertyNames()`: returns an array of own string properties
- `for...in`: loops through enumerable string properties
- `JSON.stringify()`: converts JavaScript value to a JSON string

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
- Because these operations ignore symbols, they are less discoverable compare to string properties

---

## Symbol for Privacy
- Symbols help to make properties harder to access but not impossible
    - There are APIs that can expose symbols
- Symbols make the author's intention of privacy very clear
- Separate private properties from enumerations
<!--DIV-LEFTCOLUMN-->
```javascript
let Item = (function() {
    const _price = Symbol('price');
    function Item (price) {
        this[_price] = price;
    }
    Item.prototype.getPrice = function () {
        return this[_price];
    };
    return Item;
})();
let item = new Item(10);
console.log(item.getPrice());
```
<!--DIV-RIGHTCOLUMN-->
```javascript
let Item = (function() {
    const _price = Symbol('price');
    class Item {
        constructor (price) {
            this[_price] = price;
        }
        getPrice () {
            return this[_price];
        }
    }
    return Item;
})();
let item = new Item(10);
console.log(item.getPrice());
```

---

## Symbol Across Realms
- Symbols created with `Symbol()` are local symbols
- Symbols created with `Symbol.for(key)` are global symbols
    - Global Symbol Registry
```javascript
let iframe = document.createElement('iframe');
iframe.src = String(window.location);
document.body.appendChild(iframe);
//Creates global symbol in current iframe
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
In previous slides, we covered how every symbol created by the symbol factory is unique, that is because symbols created this way are local symbols. Using the static method Symbol.for(key), we can also create global symbols in a registry called global symbol registry.
What's special about the global symbol registry is that it is cross-realms. For example, a global symbol created from one iframe is the same symbol in the existing frame.
Symbol.for with input `key` searches for symbol with the given key in the global symbol registry and returns it if found. Otherwise a new symbol gets created in the global symbol registry with this key.

---

## Symbol for Metaprogramming
- Static properties on Symbol class give developers access to some build-in symbols representing JavaScript's internal language behaviors
- Some Examples:
    - `Symbol.iterator`: `for...of`
    - `Symbol.match`: `String.prototype.match`
    - `Symbol.hasInstance`: `instanceof`
    - Checkout [MDN Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol#Well-known_symbols) documenting the complete list

```javascript
const customized = {
    [Symbol.hasInstance](lhs) {
        return typeof lhs === 'string';
    }
}
console.log("test" instanceof customized);
```

note:
The fact that symbol is unique makes it a suitable tool for metaprogramming in JavaScript.
These static symbols alsters the internals of the JavaScript engine.
