---
layout: post
title:  "Javascript: Prototype"
date:   2014-06-02 15:30:00
categories: 2014
---

### The prototype property ###

The functions in JavaScript are objects and they contain methods and properties.

Some of the methods that you are already familiar with are `apply()` and `call()` and some of the properties are `length` and `constructor`.

Another property of the function objects is `prototype`.

If you define a simple function `foo()` you can access its properties as you would do with any other object:

```javascript
function foo(a, b){return a * b;}
foo.length
// 2

foo.constructor
// Function()
```

`prototype` is a property that **gets created as soon as you define the function**. Its **initial value is an empty object**.

```javascript
typeof foo.prototype
// "object"
```

It's as if you added this property yourself like this:

```javascript
foo.prototype = {}
```

You can augment this empty object with properties and methods.

They won't have any effect of the `foo()` function itself; they'll only be used when you use `foo()` as a constructor.

---

### Adding Methods and Properties Using the Prototype ###

Inside a function invoked with `new` you have access to the value `this`, which contains the object to be returned by the constructor.

Augmenting (adding methods and properties to) this object is the way to add functionality to the object being created.

Take a look at the constructor function `Gadget()` which uses `this` to add two properties and one method to the objects it creates.

```javascript
function Gadget(name, color) {
	this.name = name;
	this.color = color;
	this.whatAreYou = function(){
		return 'I am a ' + this.color + ' ' + this.name;
	}
}
```

Adding methods and properties to the `prototype` property of the constructor function is another way to add functionality to the objects this constructor produces.

Let's add two more properties, `price` and `rating`, and a `getInfo()` method. Since `prototype` contains an object, you can just keep adding to it like this:

```javascript
Gadget.prototype.price = 100;
Gadget.prototype.rating = 3;
Gadget.prototype.getInfo = function() {
	return 'Rating: ' + this.rating + ', price: ' + this.price;
};
```

Instead of adding to the prototype object, another way to achieve the above result is to overwrite the prototype completely,
replacing it with an object of your choice:

```javascript
Gadget.prototype = {
	price: 100,
	rating: 3,
	getInfo: function() {
		return 'Rating: ' + this.rating + ', price: ' + this.price;
	}
};
```

---

### Using the Prototype's Methods and Properties ###

All the methods and properties you have added to the prototype are directly available as soon as you create a new object using the constructor.

If you create a `newtoy` object using the `Gadget()` constructor, you can access all the methods and properties already defined.

```javascript
var newtoy = new Gadget('webcam', 'black');
newtoy.name;
// "webcam"

newtoy.color;
// "black"

newtoy.whatAreYou();
// "I am a black webcam"

newtoy.price;
// 100

newtoy.rating;
// 3

newtoy.getInfo();
// "Rating: 3, price: 100"
```

It's important to note that the prototype is "live".

Objects are passed by reference in JavaScript, and therefore the prototype is not copied with every new object instance.

What does this mean in practice? It means that **you can modify the prototype at any time and all objects
(even those created before the modification) will inherit the changes.**

Let's continue the example, adding a new method to the prototype:

```javascript
Gadget.prototype.get = function(what) {
	return this[what];
};
```

Even though newtoy was created before the `get()` method was defined, newtoy will still have access to the new method:

```javascript
newtoy.get('price');
// 100

newtoy.get('color');
// "black"
```

---

### Own Properties versus prototype Properties ###

In the example above `getInfo()` used `this` internally to address the object.

It could've also used `Gadget.prototype` to achieve the same result:

```javascript
Gadget.prototype.getInfo = function() {
	return 'Rating: ' + Gadget.prototype.rating +
		   ', price: ' + Gadget.prototype.price;
};
```

What's is the difference? To answer this question, let's examine how the prototype works in more detail.

Let's again take our newtoy object:

```javascript
var newtoy = new Gadget('webcam', 'black');
```

When you try to access a property of `newtoy`, say `newtoy.name` the JavaScript engine
will look through all of the properties of the object searching for one called `name` and, if it finds it, will return its value.

```javascript
newtoy.name
// "webcam"
```

What if you try to access the `rating` property? The JavaScript engine will examine all of the properties of `newtoy` and will not find
the one called `rating`.

Then the script engine will identify the prototype of the constructor function used to create this object
(same as if you do `newtoy.constructor.prototype`).

If the property is found in the prototype, this property is used.

```javascript
newtoy.rating
// 3
```

This would be the same as if you accessed the prototype directly. Every object has a constructor property, which is a reference to the function that created the object, so in our case:

```javascript
newtoy.constructor
// Gadget(name, color)

newtoy.constructor.prototype.rating
// 3
```

Now... Every object has a constructor.
The prototype is an object, so it must have a constructor too. Which in turn has a prototype.

In other words you can do:

```javascript
newtoy.constructor.prototype.constructor
// Gadget(name, color)

newtoy.constructor.prototype.constructor.prototype
// Object price=100 rating=3
```

This might go on for a while, depending on how long the prototype chain is, but you eventually end up with the built-in `Object()` object,
which is the highest-level parent.

In practice, this means that if you try `newtoy.toString()` and `newtoy` doesn't have an own `toString()` method
and its prototype doesn't either, in the end you'll get the Object's `toString()`

```javascript
newtoy.toString()
// "[object Object]"
```

---

### Overwriting Prototype's Property with Own Property ###

As the above discussion demonstrates, if one of your objects doesn't have a certain property of its own, it can use one (if exists)
somewhere up the prototype chain.

What if the object does have its own property and the prototype also has one with the same name?
**The own property takes precedence over the prototype's.**

Let's have a scenario where a property name exists both as an own property and as a property of the prototype object:

``` javascript
function Gadget(name) {
	this.name = name;
}

Gadget.prototype.name = 'foo';
// "foo"
```

Creating a new object and accessing its `name` property gives you the object's own `name` property.

```javascript
var toy = new Gadget('camera');

toy.name;
// "camera"
```

If you delete this property, the prototype's property with the same name "shines through":

```javascript
delete toy.name;
// true

toy.name;
// "foo"
```

Of course, you can always re-create the object's own property:

```javascript
toy.name = 'camera';
toy.name;
// "camera"
```

---

### Enumerating Properties ###

If you want to list all properties of an object, you can use a `for-in` loop.

```javascript
var a = [1, 2, 3];

for (var i in a) {
	console.log(a[i]);
}
```

Arrays are objects, so you can expect that the `for-in` loop works for objects too:

```javascript
var o = {p1: 1, p2: 2};

for (var i in o) {
	console.log(i + '=' + o[i]);
}
```

This produces:

```javascript
// p1=1
// p2=2
```

There are some details to be aware of:

* Not all properties show up in a `for-in` loop. For example, the `length` (for arrays) and `constructor` properties will not show up. The properties that do show up are called `enumerable`. You can check which ones are enumerable with the help of the `propertyIsEnumerable()` method that every
object provides.
* Prototypes that come through the prototype chain will also show up, provided they are enumerable. You can check if a property is an own property versus prototype's using the `hasOwnProperty()` method.
* `propertyIsEnumerable()` will return `false` for all of the prototype's properties, even those that are enumerable and will show up in the
`for-in` loop.


Let's see these methods in action. Take this simplified version of `Gadget()`:

```javascript
function Gadget(name, color) {
	this.name = name;
	this.color = color;
	this.someMethod = function(){ return 1; }
}

Gadget.prototype.price = 100;
Gadget.prototype.rating = 3;
```

Creating a new object:

```javascript
var newtoy = new Gadget('webcam', 'black');
```

Now if you loop using a `for-in`, you see of the object's all properties, including those that come from the prototype:

```javascript
for (var prop in newtoy) {
	console.log(prop + ' = ' + newtoy[prop]);
}
```

The result also contains the object's methods (as methods are just properties that happen to be functions):

```javascript
name = webcam
color = black
someMethod = function () { return 1; }
price = 100
rating = 3
```

If you want to distinguish between the object's own properties versus the prototype's properties, use `hasOwnProperty()`. Try first:

```javascript
newtoy.hasOwnProperty('name')
// true

newtoy.hasOwnProperty('price')
// false
```

Let's loop again, but showing only own properties:

```javascript
for (var prop in newtoy) {
	if (newtoy.hasOwnProperty(prop)) {
		console.log(prop + '=' + newtoy[prop]);
	}
}
```

The result:

```javascript
name=webcam
color=black
someMethod=function () { return 1; }
```

Now let's try `propertyIsEnumerable()`. This method returns `true` for own properties that are not built-in:

```javascript
newtoy.propertyIsEnumerable('name')
// true
```

Most built-in properties and methods are not enumerable:

```javascript
newtoy.propertyIsEnumerable('constructor')
// false
```

Any properties coming down the prototype chain are not enumerable:

```javascript
newtoy.propertyIsEnumerable('price')
// false
```

Note, however, that such properties are enumerable if you reach the object contained in the prototype and invoke its propertyIsEnumerable().

```javascript
newtoy.constructor.prototype.propertyIsEnumerable('price')
// true
```

---

### isPrototypeOf() ###

Every object also gets the `isPrototypeOf()` method.

This method tells you whether that specific object is used as a prototype of another object.

Let's take a simple object `monkey`.

```javascript
var monkey = {
	hair: true,
	feeds: 'bananas',
	breathes: 'air'
};
```

Now let's create a `Human()` constructor function and set its `prototype` property to point to `monkey`.

```javascript
function Human(name) {
	this.name = name;
}

Human.prototype = monkey;
```

Now if you create a new `Human` object called `george` and ask: "Is `monkey` george's prototype?", you'll get `true`.

```javascript
var george = new Human('George');
monkey.isPrototypeOf(george)

// true
```

---

### The Secret __proto__ Link ###

As you know already, the prototype property will be consulted when you try to access a property that does not exist in the current object.

Let's again have an object called monkey and use it as a prototype when creating objects with the `Human()` constructor.

```javascript
var monkey = {
feeds: 'bananas',
breathes: 'air'
};

function Human() {}
Human.prototype = monkey;
```

Now let's create a `developer` object and give it some properties:

```javascript
var developer = new Human();
developer.feeds = 'pizza';
developer.hacks = 'JavaScript';
```

Now let's consult some of the properties. `hacks` is a property of the `developer` object.

```javascript
developer.hacks
// "JavaScript"
```

`feeds` could also be found in the object.

```javascript
developer.feeds
// "pizza"
```

`breathes` doesn't exist as a property of the `developer` object, so the prototype is looked up,
as if there is a secret link pointing to the prototype object.

```javascript
developer.breathes
// "air"
```

Can you get from the developer object to the prototype object?
Well, you could, using `constructor` as the middleman, so having something like `developer.constructor.prototype` should point to `monkey`.

The problem is that this is not very reliable, because `constructor` is more for informational purposes and can easily be overwritten at any time.
You can overwrite it with something that's not even an object and this will not affect the normal functioning of the prototype chain.

Let's set the constructor property to some string:

```javascript
developer.constructor = 'junk'
// "junk"
```

It seems like the `prototype` is now all messed up:

```javascript
typeof developer.constructor.prototype
// "undefined"
```

...but it isn't, because the `developer` still `breathes` "air":

```javascript
developer.breathes
// "air"
```

This shows that the secret link to the prototype still exists. The secret link is exposed in Firefox as the `__proto__` property.

```javascript
developer.__proto__
// Object feeds=bananas breathes=air
```

You can use this secret property for learning purposes but it's not a good idea to use it in your real scripts, because **it does not exist in Internet Explorer**, so your scripts won't be portable.

For example, let's say you have created a number of objects with `monkey` as a prototype and now you want to change something in all objects. You can change `monkey` and all instances will inherit the change:

```javascript
monkey.test = 1
// 1

developer.test
// 1
```

`__proto__` is not the same as `prototype`.
`__proto__` is a property of the instances, whereas `prototype` is a property of the constructor functions.

```javascript
typeof developer.__proto__
// "object"

typeof developer.prototype
// "undefined"
```

Once again, you should use `__proto__` only for learning or debugging purposes.

---

### Augmenting Built-in Objects ###

The built-in objects such as the constructor functions `Array`, `String`, and even `Object`, and `Function` can be augmented through
their prototypes, which means that you can, for example, add new methods to the `Array` prototype and in this way make them available
to all arrays. Let's do this.

In PHP there is a function called `in_array()` that tells you if a value exists in an array.
In JavaScript there is no `inArray()` method, so let's implement it and add it to `Array.prototype`.

```javascript
Array.prototype.inArray = function(needle) {
	for (var i = 0, len = this.length; i < len; i++) {
		if (this[i] === needle) {
			return true;
		}
	}
	
	return false;
}
```

Now all arrays will have the new method. Let's test:

```javascript
var a = ['red', 'green', 'blue'];

a.inArray('red');
// true

a.inArray('yellow');
// false
```

Now... Imagine your application often needs to reverse strings and you feel there should be a built-in `reverse()` method for string objects.
After all, arrays have `reverse()`. You can easily add this `reverse()` method to the String prototype, borrowing `Array.prototype.reverse()`.

```javascript
String.prototype.reverse = function() {
	return Array.prototype.reverse.apply(this.split('')).join('');
}
```

This code uses `split()` to create an array from a string, then calls the `reverse()` method on this array, which produces a reversed array.

The result array is turned back into a string using `join()`. Let's test the new method:

```javascript
"Stoyan".reverse();
// "nayotS"
```

---

### Some Prototype gotchas ###

Here are two interesting behaviors to consider when dealing with prototypes:

* The prototype chain is live with the exception of when you completely replace the prototype object
* `prototype.constructor` is not reliable

Creating a simple constructor function and two objects:

```javascript
function Dog(){this.tail = true;}

var benji = new Dog();
var rusty = new Dog();
```

Even after you create the objects, you can still add properties to the prototype and the objects will have access to the new properties.

Let's throw in the method `say()`:

```javascript
Dog.prototype.say = function(){return 'Woof!';}
```

Both objects have access to the new method:

```javascript
benji.say();
// "Woof!"

rusty.say();
// "Woof!"
```

Up to this point if you consult your objects, asking which constructor function was used to create them, they'll report it correctly.

```javascript
benji.constructor;
// Dog()

rusty.constructor;
// Dog()
```

Note that if you ask what is the constructor of the prototype object, you'll also get `Dog()`, which is not quite correct.
The prototype is just a normal object created with `Object()`. It doesn't have any of the properties of an object constructed with `Dog()`.

```javascript
benji.constructor.prototype.constructor
// Dog()

typeof benji.constructor.prototype.tail
// "undefined"
```

Now let's completely overwrite the prototype object with a brand new object:

```javascript
Dog.prototype = {paws: 4, hair: true};
```

It turns out that our old objects do not get access to the new prototype's properties;
they still keep the secret link pointing to the old prototype object:

```javascript
typeof benji.paws
// "undefined"

benji.say()
// "Woof!"

typeof benji.__proto__.say
// "function"

typeof benji.__proto__.paws
// "undefined"
```

Any new objects you create from now on will use the updated prototype:

```javascript
var lucy = new Dog();

lucy.say()
// TypeError: lucy.say is not a function

lucy.paws
// 4
```

The secret `__proto__` link points to the new prototype object:

```javascript
typeof lucy.__proto__.say
// "undefined"

typeof lucy.__proto__.paws
// "number"
```

Now the constructor property of the new objects no longer reports correctly. It should point to `Dog()`, but instead it points to `Object()`.

```javascript
lucy.constructor
// Object()

benji.constructor
// Dog()
```

The most confusing part is when you look up the prototype of the constructor:

```javascript
typeof lucy.constructor.prototype.paws
// "undefined"

typeof benji.constructor.prototype.paws
// "number"
```

The following would have fixed all of the unexpected behavior described above:

```javascript
Dog.prototype = {paws: 4, hair: true};
Dog.prototype.constructor = Dog;
```

**Best Practice**: When you overwrite the prototype, it is a good idea to reset the constructor property.

---

### Summary ###

* All functions have a property called `prototype`. Initially it contains an empty object.
* You can add properties and methods to the prototype object. You can even replace it completely with an object of your choice.
* When you create objects using a function as a constructor (with `new`), the objects will have a secret link pointing to their prototype, and can access the prototype's properties as their own.
* Own properties take precedence over prototype's properties with the same name.
* Use the `hasOwnProperty()` method to differentiate between own properties and prototype properties.
* There is a prototype chain: if your object foo doesn't have a property `bar`, when you do `foo.bar`, JavaScript will look for a `bar` property of the prototype. If none is found, it will keep searching in the prototype's prototype, then the prototype of the prototype's prototype and keep going all the way up to the highest-level parent `Object`.
* You can augment built-in constructor functions and all objects will see your additions. Assign a function to `Array.prototype.flip` and all arrays will immediately get a `flip()` method, `[1,2,3].flip()`. Do check if the method/property you want to add already exists, so you can future-proof your scripts.

---

[Source: "Object Oriented JavaScript"](http://www.amazon.com/Object-Oriented-JavaScript-Stoyan-Stefanov-ebook/dp/B0057UNEJC/)