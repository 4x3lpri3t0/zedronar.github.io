---
layout: post
title:  "Javascript: OOP"
date:   2014-04-16 13:30:00
categories: "2014"
---

### Object-Oriented Programming ###

Here's a list of concepts that are most often used when talking about object-oriented programming (OOP):

* Object, method, property
* Class
* Encapsulation
* Aggregation
* Reusability/inheritance
* Polymorphism

### Objects ###

An object is a representation of a *thing* (someone or something), and this representation is expressed with the help of a programming language. The thing can be anything—a real-life object, or some more convoluted concept. Taking a common object like a cat for example, you can see that it has certain characteristics (color, name, weight) and can perform some actions (meow, sleep, hide, escape). The characteristics of the object are called **properties** in OOP and the actions are called **methods**.
There is also an analogy with the spoken language:

* Objects are most often named using nouns (book, person)
* Methods are verbs (read, run)
* Values of the properties are adjectives

Let's take, for example, the sentence "The black cat sleeps on my head". "The cat"
(a noun) is the object ,"black" (adjective) is the value of the `color` property, and "sleep" (a verb) is an action, or a method in OOP. For the sake of the analogy, we can go a step further and say that "on my head" specifies something about the action "sleep", so it's acting as a **parameter** passed to the `sleep` method.

### Classes ###

In real life, similar objects can be grouped based on some criteria. A hummingbird and an eagle are both birds, so they can be classified as belonging to the Birds class. In OOP, a class is a blueprint, or recipe for an object. Another name for *object* is *instance*, so we say that the eagle is an instance of the Birds class. You can create different objects using the same class, because **a class is just a template**, while **the objects are concrete instances**, based on the template.

There's a difference between JavaScript and the "classic" OO languages like C++ and Java. You should understand right from the start that **in JavaScript there are no classes; everything is based on objects**. JavaScript has the notion of **prototypes**, which are also objects (we'll discuss them later in detail). In a classic OO language, you'd say something like "create me a new object called Bob which is of class Person". **In a prototypal OO language, you'd say, "I'm going to take this object Person that I have lying around and reuse it as a prototype for a new object that I'll call Bob".**

### Encapsulation ###

*Encapsulation* is another OOP-related concept, which illustrates the fact that an object contains (encapsulates) both:

* Data (stored in properties) and
* The means to do something with the data (using methods)

One other term that goes together with encapsulation is *information hiding*.

Imagine an object, say an MP3 player. You, as a user of the object, are given some interface to work with, such as buttons, the display, and so on. You use the interface in order to get the object to do something useful for you, like playing a song. Exactly how it is working on the inside, you don't know and, most often, don't care. In other words, the implementation of the interface is hidden from you. The same thing happens in OOP, when your code uses an object by calling its methods. It doesn't matter if you coded the object yourself or it came from some third party library; **your code doesn't need to know how the methods work internally**.

Another aspect of information hiding is the visibility of `methods` and `properties`. In some languages, objects can have *public*, *private*, and *protected* methods and properties. This categorization defines the **level of access the users of the object have**. For example, only the internal implementation of the object has access to the private methods, while anyone has access to the public ones. In JavaScript, all methods and properties are public, but there are ways to protect the data inside an object and achieve privacy.

### Aggregation ###

Combining several objects into a new one is known as *aggregation* or *composition*. The aggregation concept illustrates the ability to combine several objects into a new one. Aggregation is a powerful way to separate a problem into smaller and more manageable parts (divide and conquer). When a problem scope is so complex that it's impossible to think about it at a detailed level in its entirety, you can separate the problem into several smaller areas, and possibly then separate each of these into even smaller chunks. This allows you to think about the problem on several levels of *abstraction*.

A personal computer is a very complex object. You cannot think about all the things that need to happen when you start your computer. But you can abstract the problem saying that you need to initialize the objects it consists of: the Monitor object, the Mouse object, the Keyboard object, and so on. Then you can dive deeper into each of the sub-objects. This way you are composing complex objects by assembling reusable parts.

To use another analogy, a Book object could can contain (aggregate) one or more author objects, a publisher object, several chapter objects, a table of contents, and
so on.

### Inheritance ###

Inheritance is a very elegant way to reuse code that has already been written.

In classical OOP, classes inherit from other classes, but in JavaScript, because there are no classes, objects inherit from other objects.

When an object inherits from another object, it usually adds new methods to the inherited ones, thus *extending* the old object. Often the following phrases can be used interchangeably: "B inherits from A" and "B extends A". Also, the object that inherited a number of methods, can pick one or more methods and redefine them, customizing them for its own needs. This way the interface stays the same, the method name is the same, but when called on the new object, the method behaves differently. This way of redefining how an inherited method works is known as *overriding*.

### Polymorphism ###

Ability to call the **same method** on **different objects** and have **each of them respond in their own way**.

---

[Source: "Object Oriented JavaScript"](http://www.amazon.com/Object-Oriented-JavaScript-Stoyan-Stefanov-ebook/dp/B0057UNEJC/)