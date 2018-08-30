---

# ES6 for-of Loop

---

## Comparison
### for loop vs. forEach() vs. for-of
```javascript
const numbers = [1, 2, 3, 4, 5];
```
- for loop: can break from loop
```javascript
let sum = 0;
for (let i = 0; i < numbers.length; ++i) {
	if (sum + numbers[i] > 10) 
		break;
	else 
		sum += numbers[i];
}
```
- forEach: more concise
```javascript
let sum = 0;
numbers.forEach( (num) => {
	if (sum + num <= 10) 
		sum += num;
});
```
- for-of: combines two advantages
```javascript
let sum = 0;
for (const num of numbers) {
	if (sum + num > 10)
		break;
	sum += num;
}
```

---

## for-of
### Iterables

