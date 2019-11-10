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
* **Whenever a new request is received, the request event is called, providing two objects:   a request ::http.IncomingMessage:: object  and a response::http.ServerResponese:: object**


## V8
- - - -
1. V8 was chosen for being the engine by Node.js back 2009
- - - -

### The other JS engines

* Chrome - V8 
* Firefox - Spidermonkey
* Safari - JavascriptCore(Nitro)
* Edge - Chakra

### The quest for performance

* V8 is written in C++ and it’s continuously improved

### Compilation

* Javascript is internally compiled by V8 with **JIT** compilation

## How to exit from a node program
- - - -
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

## How to read environment variables
- - - -
1. The `process` core module of node provides the `env` property which hosts  **all the environment variables** that were set at the moment the process was started
- - - -
* `process.env.NODE_ENV`
> 	Set it to _production_ before the script runs will tell the node that is a production environment. In the same way you can access any custom environment variable you set  

## Use the node REPL
- - - -
1. ::REPL:: stands for **Read-Evaluate-Print-Loop**, and it’s a great way to explore the node features in a quick way.
- - - -

### Node commands

* Use `node` to jump into the REPL mode.
* Use the tab to autocomplete in the REPL mode.
* **Add a dot and press tab,** the REPL will print all the properties and methods you can access on the class
* Input  `> global.`  to explore global variables.
* If after some code you type `_`, that is going to print the result of last operation

### Pass arguments from the command line

- Arguments can be *standalone* or have a *key and value*

1 . 
```bash
node app.js kevin
```
```js
const args = process.argv.slice(2)
```
2 . 
```bash
node app.js name=kevin
```
```js
const args = process.argv.slice(2)
args[0]  //name=kevin
```
Use minimist library:
```js
const args = require('minimist')process.argv.slice(2)
arhs['name']  //kevin
```

## Output the Command Line

- Basic output using the console module

Almost same as `console` on the browser
> `%o` is used to print an object representation

- Clear the Console
`console.clear()`
- Counting Elements
```js 
const oranges = ['oranges','oranges'];

console.count(oranges[0]); //orange: 1
console.count(oranges[0]); //orange: 2
console.count(oranges[1]); //orange: 3
console.count(oranges[2]); //default: 1
```

- Print the Stack Trace
It tells how to reach that part of code
```js
const function2 = () => console.trace();
const function1 = () => function2();
function1();
//Trace
//		at function2 (repl:1:33)
//		at function1 (repl:1:25)
//		at repl:1:1
//		at Script.runInThisContext (vm.js:119:20)
//		at REPLServer.defaultEval (repl.js:332:29)
//		at bound (domain.js:395:14)
//		at REPLServer.runBound [as eval] (domain.js:408:12)
//		at REPLServer.onLine (repl.js:639:10)
//		at REPLServer.emit (events.js:194:15)
//		at REPLServer.EventEmitter.emit (domain.js:441:20)
```
- Calculate the Time Spent
```js
console.time(tag)
...
console.timeEnd(tag)
```
 - Stdout and Stderr
 - Color the Output
 ```js
 const chalk = require('chalk');
 console.log(chalk.yellow('it's yellow now'))
 ```
 - Create a Progress Bar
 ```js
 const ProgressBar = require('progress')
 const bar = new ProgressBar(':bar', { tatol:10 })
 const timer = setInterval(()=> {
	bar.tick()
	if(bar.complete){
		 clearInterval(timer)
	}
},100)
 ```

## Accept Input from the Command Line

- Since node version 7 provides `readline` module: **get input from a readable stream** such as the `process.stdin` stream, which during the execution of a node program is a terminal input, **one line at a time**.

```js
const readline = require('readline').createInterface({
	input: process.stdin,
	output: process.stdout
})

readline.question(`What is your name?`, name => {
	console.log(`Hi ${name}!`)
	readline.close()
})
```
The `question()` method shows the first parameter (a question) and waits for the user input. It calls the callback function once enter is pressed.

- Inquirer.js
```js
const inquirer = require('inquirer')

const question = [{
	type: 'input',
	name: 'name',
	message: "what's your name?"
}]

inquirer.prompt(questions).then(answers => {
	console.log(`Hi ${answers['name']}`)
})
```