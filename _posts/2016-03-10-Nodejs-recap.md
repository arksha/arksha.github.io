---
layout: post
title: Node.JS Recap
date: 2016-03-10
categories: web
---

##How Node.js works##
1.single threaded
	
* no blocking, single threaded

* in the cloud all behavior the same thread 

* asynchronously, more than one thing at a time 

##The global object##
	
Basic use of global object:

	console.log("hello world");
	
	global.console.log("hello world");
	//get substring
	var hello = "hello world";
	var nodejs = hello.slice(4);
	global.console.log(nodejs);
	global.console.log(`hhahahah ${nodejs}`);

Get current model and full path:

	console.log(__dirname);
	console.log(__filename);

require path model:

	var path = require("path");
	console.log(`require from ${path.basename(__filename)}`);

**we can run without .js extension, it doesn't matter**

##Process object##

1.get environment information, grab a information:

	function grab(flag) {
		var index = process.argv.indexOf(flag);
		return (index === -1) ? null : process.argv[index+1];
	}

	var greeting = grab('--greeting');
	var user = grab('--user');

	if (!user || !greeting) {
		console.log("You Blew it!");
	} else {
		console.log(`Welcome ${user}, ${greeting}`);
	}


This is a great tool if we want to specify certain ports, or specify certain file folders for our app to use at the very beginning.

2.Standard input and output(with event liseners):
	
	process.stdout.write("Hello~~\n");
	
	var questions = [
 		"What is your name?",
  		"What is your fav hobby?",
	  	"What is your preferred programming language?"
	];

	var answers = [];

	function ask(i) {
  		process.stdout.write(`\n\n\n\n ${questions[i]}`);
  		process.stdout.write("  >  ");
	}

	process.stdin.on('data', function(data) {

		answers.push(data.toString().trim());

		if (answers.length < questions.length) {
			ask(answers.length);
		} else {
			process.exit();
		}

		});

		process.on('exit', function() {

		process.stdout.write("\n\n\n\n");

		process.stdout.write(`Go ${answers[1]} ${answers[0]} you can finish writing ${answers[2]} later`);

		process.stdout.write("\n\n\n\n");

	});

	ask(0);

So process.stdin and process.stdout are ways that we can communicate with a running process.

**Control C will kill application**

3.Global timing funcions

`setTimeout ` will create a delay of a certain time and then invoke a callback function

`setInterval`where setInterval will fire the callback function over and over again on an interval time

	var waitTime = 3000;
	var currentTime = 0;
	var waitInterval = 10;
	var percentWaited = 0;

	function writeWaitingPercent(p) {
		process.stdout.clearLine();
		process.stdout.cursorTo(0);
		process.stdout.write(`waiting ... ${p}%`);
	}

	var interval = setInterval(function() {
		currentTime += waitInterval;
		percentWaited = Math.floor((currentTime/waitTime) * 100);
		writeWaitingPercent(percentWaited);
	}, waitInterval);

	setTimeout(function() {
		clearInterval(interval);
		writeWaitingPercent(100);
		console.log("\n\n done \n\n");
	}, waitTime);

	process.stdout.write("\n\n");
	writeWaitingPercent(percentWaited);
	
##Core modules##

1.util module, v8 (get memory information)
	
	var util = require('util');
	var v8 = require('v8');
	
	//also adds date and time stamp on log function
	util.log( path.basename(__filename) );
	//will give us current information on our current memory statistics
	util.log(v8.getHeapStatistics());
	
	var dirUploads = path.join(__dirname, 'www', 'files', 'uploads');

	util.log(dirUploads);

2.collect information with readline

readline instance can create a interface to manage stdin and stdout

	var rl = readline.createInterface(process.stdin, process.stdout);
create a js object realperson to store information from terminal

	var realPerson = {
		name: '',
		sayings: []
	};
`question` funtion is to ask and trigger a function for this quesion
`setPrompt` to ask the question over and over again

	rl.question("What is the name of a real person? ", function(answer) {

	realPerson.name = answer;
	
		rl.setPrompt(`What would ${realPerson.name} say? `);

		rl.prompt();

	rl.on('line', function(saying) {

		realPerson.sayings.push(saying.trim());

		if (saying.toLowerCase().trim() === 'exit') {
			rl.close();
		} else {
			rl.setPrompt(`What else would ${realPerson.name} say? ('exit' to leave) `);
		    rl.prompt();
		}

	});

	});