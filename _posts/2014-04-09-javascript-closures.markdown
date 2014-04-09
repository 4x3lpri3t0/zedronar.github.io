---
layout: post
title:  "JavaScript: Closures"
date:   2014-04-09 01:39:00
categories: javascript closures
---

# JavaScript: Closures #

Closures are functions that refer to independent variables.

The function defined in the closure 'remembers' the environment in which it was created in.

e.g.:

{% highlight javascript %}
function init() {
	var name = "Mozilla";     // local variable created by init
	function displayName() {  // inner function, a closure
		alert(name); 		  // uses variable declared in the parent function
	}
	displayName();
}
init();
{% endhighlight %}

Now consider this:

{% highlight javascript %}
function makeFunc() {
	var name = "Mozilla";
	function displayName() {
		alert(name);
	}
	return displayName;
}

var myFunc = makeFunc();
myFunc();
{% endhighlight %}

`displayName()` inner function was returned from the outer function before being executed.

Normally, the local variables within a function only exist for the duration of that function's execution. Once makeFunc() has finished executing, it is reasonable to expect that the name variable will no longer be accessible. Since the code still works as expected, this is obviously not the case.

The reason for this is that myFunc has become a closure. A closure is a special kind of object that combines two things: a function, and the environment in which that function was created



[Under construction...]

---

Check out [MDN][mdn] for more info on how closures work.

[mdn]:    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures