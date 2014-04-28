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

```javascript```
eval('var ii = 2;');
ii; // 2
```

So eval('var ii = 2;') is the same as simply var ii = 2;

eval() can be useful sometimes, but should be avoided if there are other options. Most of the time there will be alternatives and, in most cases, the alternatives are more elegant and easier to write and maintain ("Eval is evil").



---

[Source: "Object Oriented JavaScript"](http://www.amazon.com/Object-Oriented-JavaScript-Stoyan-Stefanov-ebook/dp/B0057UNEJC/)

[Source: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments)