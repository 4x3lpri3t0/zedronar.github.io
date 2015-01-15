---
layout: post
title:  "Javascript: Scope"
date:   2014-04-11 11:30:00
categories: 2014
---

Elements that interact to process the program var `a = 2;`:

**Engine**: responsible for start-to-finish compilation and execution of our JavaScript program.

**Compiler**: one of Engine's friends; handles all the dirty work of parsing and code-generation.

**Scope**: another friend of Engine; collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

To fully understand how JavaScript works, you need to begin to think like Engine (and friends) think.

### Back & Forth ###

When you see the program `var a = 2;`, you most likely think of that as one statement. But that's not how our new friend *Engine* sees it. In fact, *Engine* sees two distinct statements, one which *Compiler* will handle during compilation, and one which *Engine* will handle during execution.

So this is how *Engine* and friends will approach the program `var a = 2;`:

The first thing *Compiler* will do with this program is perform lexing to break it down into tokens, which it will then parse into a tree. But when *Compiler* gets to code-generation, it will treat this program somewhat differently than perhaps assumed.

A reasonable assumption would be that *Compiler* will produce code that could be summed up by this pseudo-code: "Allocate memory for a variable, label it `a`, then stick the value `2` into that variable." Unfortunately, that's not quite accurate.

*Compiler* will instead proceed as:

1. Encountering var `a`, *Compiler* asks *Scope* to see if a variable `a` already exists for that particular scope collection. If so, Compiler ignores this declaration and moves on. Otherwise, *Compiler* asks *Scope* to declare a new variable called a for that scope collection.

1. *Compiler* then produces code for *Engine* to later execute, to handle the `a = 2` assignment. The code *Engine* runs will first ask *Scope* if there is a variable called `a` accessible in the current scope collection. If so, *Engine* uses that variable. If not, *Engine* looks *elsewhere* (see nested *Scope* section below).

If *Engine* eventually finds a variable, it assigns the value `2` to it. If not, Engine will raise its hand and yell out an error!

**To summarize**: two distinct actions are taken for a variable assignment: **First**, *Compiler* declares a variable (if not previously declared in the current scope), and **second**, when executing, *Engine* looks up the variable in *Scope* and assigns to it, if found.

---

[Source](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20&%20closures/ch1.md)