<a id="top"></a>
# Advanced JavaScript
 
## Table of Contents
/* todo need to link the table of contents */
- asynchronous functions
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

What does this produce?  

Is it this:

```
0
1  
2  
3  
4
```

/* Maybe something here */

Actually, it produces:

```
5  
5  
5  
5  
5
```

What the heck?

setTimeout is an asynchronous function.

>asynchronous (adj.)  
>	1. not occurring at the same time.  
>	2. (of a computer or other electrical machine) having each operation started	only after the preceding operation is completed.  
>	3. Computers, Telecommunications. of or relating to operation without the use of fixed time intervals (opposed to synchronous).  












