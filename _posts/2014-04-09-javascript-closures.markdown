---
layout: post
title:  "JavaScript Closures"
date:   2014-04-09 01:39:00
categories: javascript closures
---

<html>
<head>
<link href="prism.css" rel="stylesheet" type="text/css">
</head>
<body>

Closures are functions that refer to independent variables.

i.e.:

The function defined in the closure 'remembers' the environment in which it was created in.

e.g.:

{% prism javascript %}
function init() {
	var name = "Mozilla";     // local variable created by init
	function displayName() {  // inner function, a closure
		alert(name); 		  // uses variable declared in the parent function
	}
	displayName();
}
init();
{% endprism %}


Check out the [MDN][mdn] for more info on how closures work.

[mdn]:    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures

<script src="prism.js"></script>
</body>
</html>