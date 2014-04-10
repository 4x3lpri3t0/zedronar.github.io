---
layout: post
title:  "JavaScript Closures"
date:   2014-04-09 01:39:00
categories: javascript closures
---

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

[Under construction...]

Check out [MDN][mdn] for more info on how closures work.

[mdn]:    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures