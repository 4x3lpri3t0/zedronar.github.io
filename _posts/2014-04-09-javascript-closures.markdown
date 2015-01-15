---
layout: post
title:  "JavaScript: Closures"
date:   2014-04-09 01:39:00
categories: 2014
---

Closures are functions that refer to independent variables.

The function defined in the closure 'remembers' the environment in which it was created in.

e.g.:

{% highlight javascript %}
function init() {
	var name = "I'm being printed!";     // local variable created by init
	function displayName() {  // inner function, a closure
		alert(name); 		  // uses variable declared in the parent function
	}
	displayName();
}
init();
{% endhighlight %}

---

Now consider this:

{% highlight javascript %}
function makeFunc() {
	var name = "I'm being printed!";
	function displayName() {
		alert(name);
	}
	return displayName;
}

var myFunc = makeFunc();
myFunc();
{% endhighlight %}

`displayName()` inner function was returned from the outer function before being executed.

Normally, the local variables within a function only exist for the duration of that function's execution. Once `makeFunc()` has finished executing, it is reasonable to expect that the name variable will no longer be accessible. Since the code still works as expected, this is obviously not the case.

The reason for this is that myFunc has become a *closure*. **A closure is a special kind of object that combines two things: a function, and the environment in which that function was created**.

---

A more interesting example:
 
{% highlight javascript %}
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}


var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
{% endhighlight %}

`makeAdder` is a *function factory* — it creates functions which can add a specific value to their argument. In the above example we use our function factory to create two new functions — one that adds 5 to its argument, and one that adds 10.

`add5` and `add10` are both closures. **They share the same function body definition, but store different environments.** In add5's environment, x is 5. As far as add10 is concerned, x is 10.

---

# More Closures #


### Closure #1 ###

Take a look at this function:

```javascript
function f(){
	var b = "b";
	return function(){
		return b;
	}
}
```

This function contains a variable `b`, which is local, and therefore inaccessible from the global space:

```javascript
b; // b is not defined
```

Check out the return value of `f()`: it's another function. This new function has access to its private space, to `f()`'s space and to the global space. So it can see `b`. Because `f()` is callable from the global space (it's a global function), you can call it and assign the returned value to another global variable. The result — a new global function that has access to `f()`'s private space.

```javascript
var n = f();
n(); // "b"
```

### Closure #2 ###

The final result of the next example will be the same as the previous example, but the way to achieve it is a little different. `f()` doesn't return a function, but instead it creates a new global function `n()` inside its body.
Let's start by declaring a placeholder for the global function-to-be. This is optional, but it's always good to declare your variables. Then you can define the function `f()` like this:

```javascript
var n;
function f(){
	var b = "b";
	n = function(){
		return b;
	}
}
```

Now what happens if you invoke f()?
```javascript
f();
```

A new function is defined inside `f()` and since it's missing the `var` statement, it becomes global. During definition time, `n()` was inside `f()`, so it had access to `f()`'s scope. `n()` will keep its access to `f()`'s scope, even though `n()` becomes part of the global space.

```javascript
n(); // "b"
```

### Definition and Closure #3 ###

A closure is created when a function keeps a link to its parent's scope even after the parent has returned.

When you pass an argument to a function it becomes available as a local variable. You can create a function that returns another function, which in turn returns its parent's argument.

```javascript
function f(arg) {
	var n = function(){
		return arg;
	};
	arg++;
	
	return n;
}
```

You use the function like this:

```javascript
var m = f(123);
m(); //124
```

Notice how `arg++` was incremented after the function was defined and yet, when called, `m()` returned the updated value. This demonstrates how the function binds to its scope, not to the current variables and their values found in the scope.

### Closures in a Loop ###

Here's something that can easily lead to hard-to-spot bugs, because, on the surface, everything looks normal.
Let's loop three times, each time creating a new function that returns the loop sequence number. The new functions will be added to an array and we'll return the array at the end. Here's the function:

```javascript
function f() {
	var a = [];
	var i;
	for(i = 0; i < 3; i++) {
		a[i] = function(){
			return i;
		}
	}
	
	return a;
}
```

Let's run the function, assigning the result to the array a.

```javascript
var a = f();
```

Now you have an array of three functions. Let's invoke them by adding parentheses after each array element. The expected behavior is to see the loop sequence printed out: 0, 1, and 2. Let's try:

```javascript
a[0]() // 3
a[1]() // 3
a[2]() // 3
```

Not quite what we expected. What happened here? We created three closures that all point to the same local variable `i`. Closures don't remember the value, **they only link (reference) the `i` variable and will return its current value**. After the loop, `i`'s value is 3. So all the three functions point to the same value.

(Why 3 and not 2 is another good question to think about, for better understanding the `for` loop.)

So how do you implement the correct behavior? You need three different variables. An elegant solution is to use another closure:

```javascript
function f() {
	var a = [];
	var i;
	for(i = 0; i < 3; i++) {
		a[i] = (function(x){
			return function(){
				return x;
			}
		})(i);
	}
	
	return a;
}
```

This gives the expected result:

```javascript
var a = f();
a[0](); // 0
a[1](); // 1
a[2](); // 2
```

Here, instead of just creating a function that returns `i`, you pass `i` to another self-executing function. For this function, `i` becomes the local value `x`, and `x` has a different value every time.
Alternatively, you can use a "normal" (as opposed to self-invoking) inner function to achieve the same result. The key is to use the middle function to "localize" the value of `i` at every iteration.

```javascript
function f() {
	function makeClosure(x) {
		return function(){
			return x;
		}
	}

	var a = [];
	var i;
	for(i = 0; i < 3; i++) {
		a[i] = makeClosure(i);
	}

	return a;
}
```

---


# Getter/Setter #

Imagine you have a variable that will contain a very specific range of values. You don't want to expose this variable because you don't want just any part of the code to be able to alter its value. You protect this variable inside a function and provide two additional functions — one to *get* the value and one to *set* it. The one that sets it could contain some logic to validate a value before assigning it to the protected variable.

You place both the getter and the setter functions inside the same function that contains the secret variable, so that they share the same scope:

```javascript
var getValue, setValue;
(function() {
	var secret = 0;
	getValue = function(){
		return secret;
	};
	setValue = function(v){
		secret = v;
	};
})();
```

In this case, the function that contains everything is a self-invoking anonymous function. It defines `setValue()` and `getValue()` as global functions, while the secret variable remains local and inaccessible directly.

```javascript
getValue(); // 0
setValue(123);
getValue(); // 123
```


# Iterator #

The last closure example shows the use of a closure to accomplish *Iterator* functionality.

You already know how to loop through a simple array, but there might be cases where you have a more complicated data structure with different rules as to what the sequence of values is. You can wrap the complicated "who's next" logic into an easy-to-use `next()` function. Then you simply call `next()` every time you need the consecutive value. For this example, we'll just use a simple array, and not a complex data structure.

Here's an initialization function that takes an input array and also defines a private pointer `i` that will always point to the next element in the array.

```javascript
function setup(x) {
	var i = 0;
	return function(){
		return x[i++];
	};
}
```

Calling the `setup()` function with a data array will create the `next()` function for you.

```javascript
var next = setup(['a', 'b', 'c']);
```

From there it's easy: calling the same function over and over again gives you the next element.

```javascript
next(); // "a"
next(); // "b"
next(); // "c"
```

---

Check out [MDN][mdn] for more info on how closures work.

[mdn]:    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures