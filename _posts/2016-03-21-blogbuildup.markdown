---
layout: post
title: Blog messy build up log
date: 2016-03-21
categories: web
---

##About git I don't know much

`git help`  = `man git-log`

`git help log` will show specific usage of command, type `space bar` will roll to next page

`git diff filename.html` to check difference, use`f`or `b`to move forward and backward to see whole log, it is pager, a page shows the results.

`git mv a b` to rename a file

`git ls -la` special -la operation will show the hidden dash file, any file has a . before will be hidden(like .git).

`git log -n 2` get log of 2 log

`git log --since=2016-03-21` get log since that date

`git log --until=2016-03-21` get log until that date

`git log --author="Ting"`get log by certain author

`git log --grep="keyword"`get log search commit message with keyword,**this is why good commit message is really improtant**

`git checkout -- file.html (or directory)` stay on the same branch and find this file or dir

`git reset HEAD file.html` go HEAD pointer go last commit and reset as what last happend to undo changes 

`git commit --amend -m "message"` to amend commit 

update 4/10/2016  
---
##JS Recape

[reference for this interview questions](http://lucybain.com/blog/tags/interview-questions/)

1.js event delegation

event delegation is a simple way to handel events listeners to avoid adding event listeners to specific nodes especially when child nodes are frequently changed. Event delegation can add listeners to parent rather than on child.

2.about `this` 

When talking about `this`, we have to talk about scope (作用域)，just like  [This](http://web.archive.org/web/20110725013125/http://www.digital-web.com/articles/scope_in_javascript/ ) article said: scope is a neighbor of code and effect each other. JS resolves identifiers by climbing up scope chain, locally to globally. So just like two friends are talking about their close person, not a famous but not close person. With this understading, `this` is based on scope thing. More generally, in OOP, `this` is a way to identify and refer to current working object and get properties of it. There are several scenario of using `this`.  

Calling an object's method:  

	<script type="text/javascript"> 
  		var deep_thought = { 
	    the_answer: 42, 
			ask_question: function () { 
			return this.the_answer; 
			} 
	  	}; 
		var the_meaning = deep_thought.ask_question(); 
 	</script>


in this code, When deep_thought.ask_question() is executed, JavaScript establishes an execution context for the function call, setting this to the object referenced by whatever came before the last ”.”, in this case: deep_thought.  

constructor:  

	<script type="text/javascript"> 
	  function BigComputer(answer) { 
	   this.the_answer = answer; 
	   this.ask_question = function () { 
		return this.the_answer; 
	   } 
	  } 

	  var deep_thought = new BigComputer(42); 
	  var the_meaning = deep_thought.ask_question(); 
	 </script>

On one hand, `this` meant “the new object.” On the other hand, we entered `the_question` via `deep_thought`, so while we’re executing that method, this means “whatever `deep_thought` refers to”. this is not read from the scope chain as other variables are, but instead is reset on a context by context basis.  

Function Call:

	<script type="text/javascript"> 
	  function test_this() { 
	   return this; 
	  } 
	  var i_wonder_what_this_is = test_this(); 
	 </script>

`this` defaults to reference the most global thing it can: for web pages, this is the window object.  

Event Handler:  
if the event handler is inline,  

	<script type="text/javascript"> 
	  function click_handler() { 
	   alert(this); // alerts the window object 
	  } 
	 </script> 
	 ... 
	 <button id='thebutton' onclick='click_handler()'>Click me!</button>  
	 
`this` refer to window object.  

if it is not inline,

	<script type="text/javascript"> 
	  function click_handler() { 
	   alert(this); // alerts the button DOM node 
	  } 

	  function addhandler() { 
	   document.getElementById('thebutton').onclick = click_handler; 
	  } 

	  window.onload = addhandler; 
	 </script> 
	 ... 
	 <button id='thebutton'>Click me!</button>
	 
`this` refers to the DOM element that generated the event.  

3.prototypal inheritance
	
In JavaScript, the inheritance is prototype-based. That means that there are no classes. Instead, an object inherits from another object  

`__proto__` is to find parent object, also called prototype
`this` is irrelevant to parent object
`Object.create(proto)` create object from proto
`Object.getPrototypeOf(obj)` get proto  

4.What do you think of AMD vs CommonJS  
	
AMD:Asynchronous Module Definitions

AMD takes a browser-first approach, using asynchronous behavior and backwards compatibility but doesn't have a File I/O concept. It supports objects, functions, constructors, strings, JSON and many other types of modules, running natively in the browser. It can be used to lazy load / defer the loading of dependencies. 

CommonJS on the other hand takes a server-first approach, assuming synchronous behavior, no globals and it attempts to cater for the future - server side. So, Common JS supports unwrapped modules, freeing you of the define() wrapper that AMD enforces.

5.IIFE  

IIFE:Immediately-Invoked Function Expression [about IIFE](http://benalman.com/news/2010/11/immediately-invoked-function-expression/#iife)  
`function foo(){ }();` this does not work in IIFE.

6.variable that null, undefined or undeclared

`null` is a object, `undefined` is a type of variable, means a variable has been declared but has not yet assigned with value. `undeclared` is used with no `var` , it gets created on global object(window), but will throw error 
how to check:

typeof check: `if(typeof someUndefVar == whatever)` 
can use `value = obj.prop || defaultValue` check undefinded properties
	
	if ( some_variable == null ){
	  // some_variable is either null or undefined
	}
	
7.closure

[this article explains](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work)

a closure is the local variables for a function — kept alive after the function has returned, or
a closure is a stack-frame which is not deallocated when the function returns (as if a 'stack-frame' were malloc'ed instead of being on the stack!).  
A closure in JavaScript is like keeping a copy of all the local variables, just as they were when a function exited.


8.What's a typical use case for anonymous functions?

An anonymous function is a function that was declared without any named identifier to refer to it. As such, an anonymous function is usually not accessible after its initial creation.

Use as an argument to other functions:

	setTimeout(function() {
	  alert('hello');
	}, 1000);

Above, the anonymous function is passed to setTimeout, which will execute the function in 1000 milliseconds.

Use as a closure:

	(function() {
	  alert('foo');
	})();

Breakdown of the above anonymous statements:  
The surrounding braces is a wrapper for the anonymous function  
The trailing braces initiates a call to the function and can contain arguments 

Another way to write the previous example and get the same result:  

	(function(message) {
	  alert(message);
	}('foo'));
	
9.module pattern, classical inheritance

[this blog helps](http://metaduck.com/08-module-pattern-inheritance.html)

10.What's the difference between host objects and native objects?

Host objects:   
  Everything the environment gives you. For the browser, this includes objects like `window`.  
Native Objects:
  Native objects are inherent to JS  
User objects:
	User objects are anything the user defines. When you create a new object that is not directly a native object, you've made a user object.  
	
11. Difference between: `function Person(){}`, var `person = Person()`, and `var person = new Person()`

`function Person(){}`:  
this defines a constructor, because in JS, a function start with capital letter usually indicates this is a constructor.  

`person = Person()`:  
This is a mistake. There are ways of dealing with mistakes like this (one way is the "use strict" method), but ultimately this should be corrected. 

`var person = new Person()`:
In traditional OOP this may refer to a new instance of `person` class, BUT in JS, using this method `person` will have access to everything `Person.prototype` has access to, as well as any instance variables set in the `Person` constructor.

12.Function.prototype.bind

The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

**always binding `this` will lose access to inner `this`**

When would you use document.write()?
What's the difference between feature detection, feature inference, and using the UA string?
Explain Ajax in as much detail as possible.
What are the advantages and disadvantages of using Ajax?
Explain how JSONP works (and how it's not really Ajax).
Have you ever used JavaScript templating?
If so, what libraries have you used?
Explain "hoisting".
Describe event bubbling.
What's the difference between an "attribute" and a "property"?
Why is extending built-in JavaScript objects not a good idea?
Difference between document load event and document DOMContentLoaded event?
What is the difference between == and ===?
Explain the same-origin policy with regards to JavaScript.

Make this work:
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]

Why is it called a Ternary expression, what does the word "Ternary" indicate?
What is "use strict";? what are the advantages and disadvantages to using it?
Create a for loop that iterates up to 100 while outputting "fizz" at multiples of 3, "buzz" at multiples of 5 and "fizzbuzz" at multiples of 3 and 5


Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
Why would you use something like the load event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
Explain what a single page app is and how to make one SEO-friendly.
What is the extent of your experience with Promises and/or their polyfills?
What are the pros and cons of using Promises instead of callbacks?
What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?
What tools and techniques do you use debugging JavaScript code?
What language constructions do you use for iterating over object properties and array items?
Explain the difference between mutable and immutable objects.
What is an example of an immutable object in JavaScript?
What are the pros and cons of immutability?
How can you achieve immutability in your own code?
Explain the difference between synchronous and asynchronous functions.
What is event loop?
What is the difference between call stack and task queue?
Explain the differences on the usage of foo between function foo() {} and var foo = function() {}


##HTML recap

What does a doctype do?
What's the difference between full standards mode, almost standards mode and quirks mode?
What's the difference between HTML and XHTML?
Are there any problems with serving pages as application/xhtml+xml?
How do you serve a page with content in multiple languages?
What kind of things must you be wary of when design or developing for multilingual sites?
What are data- attributes good for?
Consider HTML5 as an open web platform. What are the building blocks of HTML5?
Describe the difference between a cookie, sessionStorage and localStorage.
Describe the difference between <script>, <script async> and <script defer>.
Why is it generally a good idea to position CSS <link>s between <head></head> and JS <script>s just before </body>? Do you know any exceptions?
What is progressive rendering?
Have you used different HTML templating languages before?


##CSS recap

What is the difference between classes and IDs in CSS?
What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?
Describe Floats and how they work.
Describe z-index and how stacking context is formed.
Describe BFC(Block Formatting Context) and how it works.
What are the various clearing techniques and which is appropriate for what context?
Explain CSS sprites, and how you would implement them on a page or site.
What are your favourite image replacement techniques and which do you use when?
How would you approach fixing browser-specific styling issues?
How do you serve your pages for feature-constrained browsers?
What techniques/processes do you use?
What are the different ways to visually hide content (and make it available only for screen readers)?
Have you ever used a grid system, and if so, what do you prefer?
Have you used or implemented media queries or mobile specific layouts/CSS?
Are you familiar with styling SVG?
How do you optimize your webpages for print?
What are some of the "gotchas" for writing efficient CSS?
What are the advantages/disadvantages of using CSS preprocessors?
Describe what you like and dislike about the CSS preprocessors you have used.
How would you implement a web design comp that uses non-standard fonts?
Explain how a browser determines what elements match a CSS selector.
Describe pseudo-elements and discuss what they are used for.
Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
What does * { box-sizing: border-box; } do? What are its advantages?
List as many values for the display property that you can remember.
What's the difference between inline and inline-block?
What's the difference between a relative, fixed, absolute and statically positioned element?
The 'C' in CSS stands for Cascading. How is priority determined in assigning styles (a few examples)? How can you use this system to your advantage?
What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
Have you played around with the new CSS Flexbox or Grid specs?
How is responsive design different from adaptive design?
Have you ever worked with retina graphics? If so, when and what techniques did you use?
Is there any reason you'd want to use translate() instead of absolute positioning, or vice-versa? And why?