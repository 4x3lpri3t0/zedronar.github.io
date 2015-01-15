---
layout: post
title:  "Javascript: Some facts"
date:   2014-04-16 15:30:00
categories: "2014"
---

### Falsey values ###

* `""`
* `null`
* `undefined`
* `0`
* `NaN`
* `false`

### Primitive Data Types ###

Any value that you use is of a certain type. In JavaScript, there are the following *primitive data types*:

1. **number** - this includes floating point numbers as well as integers, for example 1, 100, 3.14.

1. **string** - any number of characters, for example "a", "one", "one 2 three".

1. **boolean** - true/false

1. **undefined** - when you try to access a variable that doesn't exist, you get the special value undefined. The same will happen when you have declared a variable, but not given it a value yet. JavaScript will initialize it behind the scenes, with the value *undefined*.

1. **null** - this is another special data type that can have only one value, the *null* value. It means no value, an empty value, nothing. *The difference with undefined is that if a variable has a value null, it is still defined, it only happens that its value is nothing*.


### Undefined and null ###

You get the `undefined` value when you try to use a variable that **doesn't exist**, or one that **hasn't yet been assigned a value**. When you declare a variable without initializing it, JavaScript automatically initializes it to the value undefined.

The `null` value, on the other hand, is not assigned by JavaScript behind the scenes; it **can only be assigned by your code**.

---

[Source: "Object Oriented JavaScript"](http://www.amazon.com/Object-Oriented-JavaScript-Stoyan-Stefanov-ebook/dp/B0057UNEJC/)