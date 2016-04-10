---
layout: post
title: Blog messy build up log
date: 2016-03-21
categories: web
---

##About git I don't know much##

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


##JS Recape##

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

	
