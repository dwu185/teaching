# ES6 Block Scope

===

# Overview
- ES6 adds Block Scope
- `const` and `let`

note: 
ES6 has a new type of scope: Block Scope. Before ES6, the language only has function scope.
Two new keywords `const` and `let` can be used to declare block scoped variables 

---

## `var` vs. `const` `let`
### Scope Difference
- `var` is functional-scoped
- `const` and `let` are block-scoped
left column
```javascript
function test_var () {
	var test = "var declared";
	console.log(test);
}
> test_var();
< var declared 
```
right column
```javascript
function test_let () {
	{
		let test = "let declared";
	}
	console.log(test);
}
> test_let()
< Uncaught ReferenceError: test is not defined
```

note:
Before ES6, we can only use keyword `var` to declare a variable.
Now, we can also use const and let.
As mentioned previously, the main difference is the scope of the newly created variable.

---

## Block Scope
### Replace IIFEs
left column
```javascript
Use overview example
```
right column
```javascript
```

note:
Before ES6 block scope, you had to use the IIFE pattern in order to restrict the scope of a variable.
Now you can directly use blocks combined with const and let to restrict the scope.

---

## `var` vs. `const` `let`
### Strictness
- `const` and `let` are more strict
	- Exception Throwing
- `var` creates global variables, `const` and `let` do not

---

## `var` vs. `const` `let`
### Hoisting
- `var` is hoisted
	- When scope of a var variable is entered, it's initialized with `undefined`
- `const` and `let` are not hoisted 
	- When scope of a const/let variable is entered, it is not accessible until we reach declaration.
left column
```javascript
function test () {
	console.log("test = " + test);
	var test = "var declared";
}
> test();
< test = undefined
```
right column
```javascript
function test () {
	let test = "out of block";
	{
		console.log("test = " + test);
		let test = "in the block";
	}
}
> test()
< Uncaught ReferenceError: test is not defined
```

note:
For the var example, we can roughly interpret it as:
```javascript
function test () {
	var test;
	console.log("test = " + test);
	test = "var declared";
}
```
For let example:
function test () {
	{
		console.log("test = " + test);
		let test = "in the block";
	}
}
But this won't crash:
```javascript
function test () {
	let test = "out of block";
	{
		console.log("test = " + test);
	}
}
> test()
< "out of block"
```
The part of code where a const/let variable is not accessible is called temporal dead zone or TDZ.
Between entering the block and let/const variable is declared.

---

## `var` vs. `let`
### Looping
- var variable is declared once for the entire loop
- let variable is declared once for each iteration
Left column
```javascript
const funcs = [];
For (var i = 0; i < 3; ++i) {
	funcs.push(() =>  console.log(i))
}
> funcs.map(f => f());
< 3
3
3
```
Right column
```javascript
const funcs = [];
for (let i = 0; i < 3; ++i) {
	funcs.push(() =>  console.log(i))
}
> funcs.map(f => f());
< 0
1
2
```
- Do not use const to declare i

note:
With for loops, you can declare variables in the loop head. 
If you use var…
You cannot use const to declare i, we’ll explain why.
---

## `const`
- `const`: variables cannot be re-assigned
```javascript
> const age = 10;
> age = 11;
< Uncaught TypeError: Assignment to constant variable.
```
- However,  `const` doesn't make the value immutable
```javascript
> const personal_info = {age: 8, name: 'Charlie'};
> personal_info.age = 9;
> console.log(personal_info)
< {age: 9, name: 'Charlie'}
```
- `const` variables must be immediately initialized

note:
`const` works similar to `let`, but it has some special qualities.
Though const variables cannot be re-assigned to a new value.
const doesn't prevent any mutation to the assigned value.	
