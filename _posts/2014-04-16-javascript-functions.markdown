---
layout: post
title:  "Javascript: Functions"
date:   2014-04-16 19:00:00
categories: javascript
---

### arguments array-like object ###

You can create functions that are flexible about the number of parameters they accept. This is possible thanks to the `arguments` array-like object that is created automatically inside each function.

Here's a function that simply returns whatever parameters are passed to it:

```javascript
function args() { return arguments; }

args(); // []

args( 1, 2, 3, 4, true, 'ninja'); // [1, 2, 3, 4, true, "ninja"]
```

By using the arguments array-like object you can make a `sumOnSteroids()` function to accept any number of parameters and add them all up.

```javascript
function sumOnSteroids() {
	var res = 0;
	var number_of_params = arguments.length;

	for (var i = 0; i < number_of_params; i++) {
		res += arguments[i];
	}

	return res;
}

sumOnSteroids(1, 1, 1); // 3

sumOnSteroids(1, 2, 3, 4); // 10
```

Technically the `arguments` object is not an `Array` but an array-like object. It is similar to an `Array`, but does not have any `Array` properties except `length`.

### Pre-defined Functions ###

Functions that are built into the JavaScript engine and available for you to use:

* parseInt()
* parseFloat()
* isNaN()
* isFinite()
* encodeURI()
* decodeURI()
* encodeURIComponent()
* decodeURIComponent()
* eval()

### parseInt() ###

Takes any type of input (most often a string) and tries to make an integer out of it. If it fails, it returns `NaN`.

### parseFloat() ###

Same as `parseInt()` but it also looks for decimals when trying to figure out a number from your input.

### isNaN() ###

Using `isNaN()` you can check if an input value is a valid number that can safely be used in arithmetic operations.

This function is also a convenient way to check whether `parseInt()` or `parseFloat()` succeeded.

```javascript
isNaN(NaN) // true
isNaN(123) // false
isNaN(1.23) // false
isNaN(parseInt('abc123')) // true
```

### isFinite() ###

Checks whether the input is a number that is neither `Infinity` nor `NaN`.

### Encode/Decode URIs ###

In a URL (Uniform Resource Locator) or a URI (Uniform Resource Identifier), some characters have special meanings.

If you want to "escape" those characters, you can use the functions `encodeURI()` or `encodeURIComponent()`.

The first one will return a usable URL, while the second one assumes you're only passing a part of the URL, like a query string for example, and will encode all applicable characters.

### eval() ###

Takes a string input and executes it as JavaScript code:

```javascript
eval('var ii = 2;');
ii; // 2
```

So eval('var ii = 2;') is the same as simply var ii = 2;

eval() can be useful sometimes, but should be avoided if there are other options. Most of the time there will be alternatives and, in most cases, the alternatives are more elegant and easier to write and maintain ("Eval is evil").

### Functions that return Functions ###

A function always returns a value, and if it doesn't do it explicitly with return, then it it does so implicitly by returning `undefined`.
A function can return only one value and this value could be another function.

```javascript
function a() {
	alert('A!');
	return function(){
		alert('B!');
	};
}
```

In this example the function a() does its job (says A!) and returns another function that does something else (says B!). You can assign the return value to a variable and then use this variable as a normal function:

```javascript
var newFunc = a();
newFunc();
```

Here the first line will alert A! and the second will alert B!.

If you want to execute the returned function immediately, without assigning it to a new variable, you can simply use another set of parentheses. The end result will be the same.

```javascript
a()();
```

### Function, rewrite yourself ###

Because a function can return a function, you can use the new function to replace the old one.

You can take the value returned by the call to `a()` to overwrite the actual `a()` function:

```javascript
a = a();
```

The above alerts A!, but the next time you call `a()` it alerts B!.

This is useful when a function has some initial one-off work to do. The function overwrites itself after the first call in order to avoid doing unnecessary repetitive work every time it's called.

In the example above, we redefined the function from the outside, we got the returned value and assigned it back to the function. But the function can actually rewrite itself from the inside:

```javascript
function a() {
	alert('A!');
	a = function(){
		alert('B!');
	};
}
```

If you call this function for the first time, it will:

* Alert A! (consider this as being the one-off preparatory work)
* Redefine the global variable a, assigning a new function to it

Every subsequent time that the function is called, it will alert B!

---

Example that combines several of the techniques discussed:

```javascript
var a = function() {
	function someSetup(){
		var setup = 'done';
	}
	function actualWork() {
		alert('Worky-worky');
	}
	someSetup();
	return actualWork;
}();
```

In this example:

- You have private functions `someSetup()` and `actualWork()`.
- You have a self-invoking function, function `a()` calls itself using the parentheses following its definition.

The function executes for the first time, calls `someSetup()` and then returns a reference to the variable `actualWork`, which is a function.

Notice that there are no parentheses in the return, because it is returning a function reference, not the result of invoking this function.

Because the whole thing starts with `var a =...`, the value returned by the self-invoked function is assigned to a.

What will the code above alert when:

1. It is initially loaded?
1. You call a() afterwards?

These techniques could be really useful when working in the browser environment.
Because different browsers can have different ways of achieving the same thing and you know that the browser features don't change between function calls, you can have a function determine the best way to do the work in the current browser, then redefine itself, so that the "browser feature sniffing" is done only once.

---

### Interesting Array Methods ###

In addition to the methods inherited from the parent object, array objects also have some more useful methods, such as `sort()`, `join()`, and `slice()`, among others.

Let's take one array and experiment with some of these methods:

```javascript
var a = [3, 5, 1, 7, 'test'];
```

The `push()` method appends a new element at the end of the array. The `pop()` method removes the last element. `a.push('new')` works just like `a[a.length] = 'new'` and `a.pop()` is the same as `a.length--`.

`push()` returns the length of the changed array, `pop()` returns the element that it removed.

```javascript
>>> a.push('new') // 6

>>> a // [3, 5, 1, 7, "test", "new"]

>>> a.pop() // "new"

>>> a // [3, 5, 1, 7, "test"]
```

The `sort()` method sorts the array and returns the modified array. In the next example, after the sort, both `a` and `b` contain pointers to the same array.

```javascript
>>> var b = a.sort();

>>> b // [1, 3, 5, 7, "test"]

>>> a // [1, 3, 5, 7, "test"]
```

`join()` returns a string containing the values of all the elements in the array, concatenated together using the string parameter passed to `join()`

```javascript
>>> a.join(' is not '); // "1 is not 3 is not 5 is not 7 is not test"
```

`slice()` returns a piece of the array without modifying the source array. The first parameter to `slice()` is the **start index** and the second is the **end index** (both indices are zero-based).

```javascript
>>> b = a.slice(1, 3); // [3, 5]

>>> b = a.slice(0, 1); // [1]

>>> b = a.slice(0, 2); // [1, 3]
```

After all the slicing, the source array is still the same:

```javascript
>>> a // [1, 3, 5, 7, "test"]
```

`splice()` modifies the source array. It removes a slice, returns it, and optionally fills the gap with new elements. The first two parameters define start and end of the slice to be removed; the other parameters pass the new values.

```javascript
>>> b = a.splice(1, 2, 100, 101, 102); // [3, 5]

>>> a // [1, 100, 101, 102, 7, "test"]
```

Filling the gap with new elements is optional and you can skip it:

```javascript
>>> a.splice(1, 3)
[100, 101, 102]
>>> a
[1, 7, "test"]
```

---

### Function ###

Functions are actually objects. There is a built-in constructor function called `Function()` which allows an alternative (but not recommended) way to create a function.

---

[Source: "Object Oriented JavaScript"](http://www.amazon.com/Object-Oriented-JavaScript-Stoyan-Stefanov-ebook/dp/B0057UNEJC/)

[Source: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments)