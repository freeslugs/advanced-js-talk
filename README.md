<a id="top"></a>
# Advanced JavaScript
 
## Table of Contents
/* todo need to link the table of contents */
- asynchronous functions
  - synchronous functions
  - asynchronous functions
  - callbacks
- promises
- the async lib
 
-------------------------
 
<a id="async"></a>
# 1.0 Asynchronous Functions

Let's look at a snippet of code:

```javascript
for(var i = 0; i < 5; i++) {  
	setTimeout(function() {  
		console.log(i);  
	}, 1000);  
}
```

Let's run it in the javascript console and see what it produces. 

What we want: 

```
0
1  
2  
3  
4
```

In actuality it produces:

```
5  
5  
5  
5  
5
```

What the heck?

<b>setTimeout is an asynchronous function.</b>

>asynchronous (adj.)  
>	1. not occurring at the same time.  
>	2. (of a computer or other electrical machine) having each operation started	only after the preceding operation is completed.  
>	3. Computers, Telecommunications. of or relating to operation without the use of fixed time intervals (opposed to synchronous).  

That doesn't really help.

Essentially, asynchronous means <i>a function takes unpredictable time to complete.</i> 
So in our example, the for loop executed and completed before the setTimeout function even began. We need to wait for the console.log before we increment the i. How can we fix that?

## Synchronous Functions

How are functions normally defined? How do we return values?

Here's one way:

```javascript
function hello(param) {
	// do something
	// optionally return something
	return "hello!";
}
```

Here's another way:

```javascript
var hello = function(param) {
	return "hello!";
}
```

How do we call functions?

```javascript
hello("this is a string");
hello();
```

<i>Fun fact:</i> This is an anonymous function. It is not named.

```javascript
function() {
	// do something
	console.log("hello world");
}
```

Ok. So how does this differ from asynchronous functions?

## Asynchronous functions & Callbacks

Well, it’s key to understand how javascript executes code. Let’s take a look at our first example. Line numbers have been added.

```javascript
1. 	for(var i = 0; i < 5; i++) {
2. 		setTimeout(function() { 
3. 			console.log(i); 
4.		}, 1000);
5.	}
```

So first it’ll hit line 1. i is initially defined as 0, and it’ll start the loop. 
Next it’ll hit line 2 and wait a second (1000 milliseconds).

Since setTimeout is an asynchronous function, we’re actually passing in to it an <i>anonymous function</i> which is defined inline. That function is:

```javascript
function() {
	console.log(i);
}
```

This code can also be written without an anonymous function. We'll name the previous anonymous function `printSomething`.

```javascript
function printSomething() { 
	console.log(i); 
}
setTimeout(printSomething, 1000);
```

Exactly! We aren't <i>calling</i> `printSomething`, we're only defining it above, and passing it into `setTimeout` as a param. `setTimeout` will call the function. 

So let's continue with the code execution. Once it hits line 2, it'll call the setTimeout function and skip all the way to line 5. And since it's a for loop, it'll increment i and return to line 2.

It'll keep repeating until i == 5. So who executes our anonymous function?
setTimeout does. Although it's difficult to see in the native code, essentially when setTimeout finishes (it waits 1000 ms), it will call the function we just passed in.

Our anonymous function is called a callback function. /* add bold */

So let’s look at another example of a callback function. This time we’ll add some jQuery too:

```javascript
$( "#target" ).click(function() {
	alert( "Handler for .click() called." );
});
```

In this snippet of code, when you click on an element with id "target," it will alert "...".

Another example of a callback function is the jQuery GET request:

```javascript
$.get(“http://www.google.com”, function(data) {
	console.log(data);
});
```

























