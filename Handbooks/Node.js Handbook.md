# Node.js Handbook Notes

## Introduction to node
- - - -
1. Node is the server-side Javascript  runtime environment
2. Node is built on top of v8 engine
3. Node is mainly used to create servers, but it’s not limited to that
- - - -

### The best features of Node.js

* **Fast**
> _non-blocking paradigm_  
* **Simple**
* **Javascript**
* **Asynchronous platform**
> _single thread_  
> _callback function_  
> _event-driven programming_  
> 	When Node.js needs to perform an I/O operation, instead of blocking the thread, node will simply resume the operation when the response comes back.  
* **A huge number of libraries**

### An example Node.js Application

* **::createServer()::  method of http creates a new http server and  returns it**
* **Whenever a new request is received, the request event is called, providing two objects:   a request ::http.IncomingMessage:: object  and a response ::http.ServerResponese:: object**