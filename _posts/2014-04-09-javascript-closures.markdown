---
layout: post
title:  "JavaScript: Closures"
date:   2014-04-09 01:39:00
categories: javascript
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



---

Check out [MDN][mdn] for more info on how closures work.

[mdn]:    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures