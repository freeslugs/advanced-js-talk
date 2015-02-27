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

So let's continue with the code execution. Once it hits line 2, it'll call the setTimeout function and skip all the way to line 5. And since it's a for loop, it'll increment i and return to line 2. It'll keep repeating until `i == 5`. 

So who executes our anonymous function (or the `printSomething` function)?
`setTimeout` does. Although it's difficult to see in the native code, essentially when `setTimeout` completes (it waits 1000 ms), it will call the function we just passed in.

Our anonymous function then, is called a <b>callback function.</b>

So let’s look at another example of a callback function. This time we’ll add some jQuery too:

```javascript
$( "#target" ).click(function() {
	alert("Handler for .click() called");
});
```
In this snippet of code, when you click on an element with id "target", it will alert "Handler for .click() called".

Another example of a callback function is the jQuery `GET` request:

```javascript
$.get("http://www.google.com", function(data) {
	console.log(data);
});
```

Once it completes the get request, it'll call our anonymous function, which will `console.log` the data returned from the request.

Now let's make our own callback function.
Let’s say we want to make a function that retrieves data from an API, modifies the data, and returns it.

Here's the wrong way to do it:

```javascript
function getIP() {
	$.get("http://ip.jsontest.com", function(data) {
		var result = data.ip;
		return result;
	});
}
```

function(data) {
	var result = data.ip;
	return result;
}


If we were to try it out, we'd see that it returns undefined. That’s because `$.get` is asynchronous, so let's try again using a callback function. 

```javascript
function getIP(callback) {
	$.get("http://ip.jsontest.com", function(data) {
		var result = data.ip;
		callback(result):
	});
}
```

To call this function:

```javascript
getIP(function(ip) {
	console.log(ip);
});
```

Output: `127.0.0.1`

Cool!

There are some benefits to using asynchronous functions. To demonstrate this, we'll build an app that retrieves data from an API and displays it to the user.

Let's begin with a basic HTML page:

```html
<html>
<script></script>
<body>
</body>
</html>
```

Now let's make a request to the API and display the data:

```html
<html>
<script>
// when the page is loaded
$( document ).ready(function() {
	$.get("https://api.github.com/repos/octokit/octokit.rb", function(data) {
		$("#github-repo-name").text(data.full_name + " – " + data.description);
		$("#github-repo-url").text(data.html_url);
		$("#github-repo-url").attr("href", data.html_url);
	});
});
</script>
<body>
	<p id="github-repo-name"></p>
	<a id="github-repo-url"></a>
</body>
</html>
```

Now let's show a loading gif, and then hide it when the data appears:

```html
<html>
<script>
	// when the page is loaded
	$( document ).ready(function() {
		$.get("https://api.github.com/repos/octokit/octokit.rb", function(data) {
			$(“#loading-gif”).hide();
			$("#github-repo-name").text(data.full_name + " – " + data.description);
			$("#github-repo-url").text(data.html_url);
			$("#github-repo-url").attr("href", data.html_url);
		});
	});
</script>
<body>
	<img id="loading-gif" src="http://www.ajaxload.info/cache/FF/FF/FF/00/00/00/1-0.gif">
	<p id="github-repo-name"></p>
	<a id="github-repo-url"></a>
</body>
</html>
```

To emphasize this loading effect, we can add a small setTimeout:

```html
// when the page is loaded
<html>
<script>
	$( document ).ready(function() {
    		$.get("https://api.github.com/repos/octokit/octokit.rb", function(data) {
        		setTimeout(function(){ 
            			$("#loading-gif").hide();
            			$("#github-repo-name").text(data.full_name + " – " + data.description);
            			$("#github-repo-url").text(data.html_url);
            			$("#github-repo-url").attr("href", data.html_url);
            		}, 1000);
        	});
    	});
</script>
<body>
	<img id="loading-gif" src="http://www.ajaxload.info/cache/FF/FF/FF/00/00/00/1-0.gif">
	<p id="github-repo-name"></p>
	<a id="github-repo-url"></a>
</body>
</html>
```

So as you can see, callbacks are definitely useful. However, one problem that exists is <b>callback hell.</b>

Code tends to shift rightwards instead of downwards: [Source: callbackhell.com](callbackhell.com)

```javascript
fs.readdir(source, function(err, files) {
	if (err) {
		console.log('Error finding files: ' + err)
	} else {
		files.forEach(function(filename, fileIndex) {
			console.log(filename)
			gm(source + filename).size(function(err, values) {
				if (err) {
					console.log('Error identifying file size: ' + err)
				} else {
					console.log(filename + ' : ' + values)
					aspect = (values.width / values.height)
					widths.forEach(function(width, widthIndex) {
						height = Math.round(width / aspect)
						console.log('resizing ' + filename + 'to ' + height + 'x' + height)
						this.resize(width, height).write(destination + 'w' + width + '_' + filename, function(err) {
							if (err) console.log('Error writing file: ' + err)
						})
					}.bind(this))
				}
			})
		})
	}
})
```

There are a few ways to get around this. One way is promises.

# Promises

"A promise is an object that represents the return value or the thrown exception that the function may eventually provide."
[Source: https://github.com/kriskowal/q](https://github.com/kriskowal/q)

For the purposes of this talk, we are going to use the Q library.

Promises have 3 key properties:

1. then() - this function is called after the promise resolves. Using this function, we can chain together asynchronous functions seamlessly with promises.
2. resolve() - the promise succeeds and returns a value.
3. reject() - the promise fails and returns an error.

How to use a promise with an asynchronous function:

1. We first obtain a promise object by calling Q.defer().
2. We do some asynchronous processes (e.g. HTTP GET request).
3. We return the promise at the end of the function.
4. After the asynchronous processes finish, we either resolve() or reject() the promise. We do this generally in the callback function of the asynchronous function.

In order to demonstrate how to do this, we'll build an app that makes 3 GET requests using promises.

http://jsfiddle.net/mLgzx196/

```javascript
// src:  https://gist.github.com/jeffcogswell/8257755
 
function one() {
    var deferred = Q.defer();
    console.log("Starting one's's ajax");
    $.ajax( {
        url: 'http://ip.jsontest.com/',
        success: function() {
            console.log('Finished with one. Ready to call next.');
            deferred.resolve();
            console.log("hello");
            console.log(deferred);
        }
    });
    return deferred.promise;
}
 
function two() {
    var deferred = Q.defer();
    console.log("Starting two's ajax");
    $.ajax( {
        url: 'http://ip.jsontest.com/',
        success: function() {
            console.log('Finished with two. Ready to call next.');
            deferred.resolve();
        }
    });
    return deferred.promise;
}
 
function three() {
    var deferred = Q.defer();
    console.log("Starting three's ajax");
    $.ajax( {
        url: 'http://ip.jsontest.com/',
        success: function() {
            console.log('Finished with three. Ready to call next if there is one.');
            deferred.resolve();
        }
    });
    return deferred.promise;
}
 
one()
    .then(two)
    .then(three)
    .done();
```

