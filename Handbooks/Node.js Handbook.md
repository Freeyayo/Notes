# Node.js Handbook Notes

## Introduction to node
---
1. Node is the server-side Javascript  runtime environment
2. Node is built on top of v8 engine
3. Node is mainly used to create servers, but it’s not limited to that
---

### The best features of Node.js

* *Fast*
> /non-blocking paradigm/
* *Simple*
* *Javascript*
* *Asynchronous platform*
> /single thread/
> /callback function/
> /event-driven programming/
> 	When Node.js needs to perform an I/O operation, instead of blocking the thread, node will simply resume the operation when the response comes back.
* *A huge number of libraries*

### An example Node.js Application

* *::createServer()::  method of http creates a new http server and  returns it*
* *Whenever a new request is received, the request event is called, providing two objects:   a request ::http.IncomingMessage:: object  and a response::http.ServerResponese:: object*


## V8
---
1. V8 was chosen for being the engine by Node.js back 2009
---

### The other JS engines

* Chrome - V8 
* Firefox - Spidermonkey
* Safari - JavascriptCore(Nitro)
* Edge - Chakra

### The quest for performance

* V8 is written in C++ and it’s continuously improved

### Compilation

* Javascript is internally compiled by V8 with *JIT* compilation

## How to exit from a node program
---
* `ctrl-c`
* `process.exit()`
> 	When node runs this line, any callbacks that’s pending, any network request still been sent, any filesystem access, or processes writing to `stdout` or `stderr` — all is going to be ungracefully terminated right away.
>   You can pass an integer that signals the operation system the exit code
> `process.exit(1)` or `process.exitCode = 1`[more info about exit code](https://nodejs.org/dist/latest-v12.x/docs/api/process.html#process_process_exit_code)
* `res.send('Hi!')`
* SIGTERM
> `process.kill(process.pid, 'SIGTERM')`
> 
```javascript
process.on('SIGTERM', () => {
	server.close(() => {
		//...
	})
}) 
```