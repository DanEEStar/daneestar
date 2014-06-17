# Async JavaScript

[Amazon Link](http://www.amazon.com/Async-JavaScript-Responsive-Pragmatic-Express-ebook/dp/B00AKM4RVG/)

## Chapter 1

### 1.1 Scheduling Events

JavaScript event handlers don't run untl the thread is free.
The following code will always output a time elapsed of at least 1000!

```
var start = new Date; 
setTimeout(function(){
	var end = new Date;
	console.log('Time elapsed:', end - start, 'ms'); 
}, 500);
while (new Date - start < 1000) {};
```
Async callbacks are run when the current function is done running and has returned.

### 1.2 Types of Async Functions

`setTimeout` and `setInterval` have minimum delay of 4ms as specified in the [HTML5 spec](http://www.whatwg.org/specs/web-apps/current-work/multipage/timers.html#timers). Use `requestAnimationFrame` to overcome this problem.

### 1.3 Writing Async Functions

Typically functions that take a callback take it as their last argument. (not `setTimeout` and `setInterval`)

It can happen that a function is sometimes async and sometimes sync. For example when the result gets cached after the first invoke and returned immediately in every subsequent call. In this case the sync call should be wrapped in a `setTimeout`

```
if (socket.readyState === WebSocket.OPEN) { 
	setTimeout(callback, 0);} else { 
	// ...}
```

### 1.4 Handling Async Errors

`try/catch` blocks do not work with async functions! Often an additional error-Callback is used to log errors.