# Node Js
### 1. What is Node.js? ###
**Node.js** is a runtime environment that  you run JavaScript code outside of a web browser,
Node js is a single thraded non blocking architecture 
Javascript code run on server

### 2. Create a "hello Word" in Node.js ###  
```javascript
const http = require('http');  
http.createServer(function(req,res){  
  res.writeHead(200, { 'Content-Type': 'text/html' })  
  res.send('Hello world'); // Changed .send() to .end()
}).listen(8080)  
```
### 2. What is cluster in node js and why we use ###
**Clusters** of Node.js processes can be used to run multiple instances of Node.js that can distribute workloads among their application threads
Note > we are distribute workloads in multiple workers
```javascript
const cluster = require('node:cluster');
const http = require('node:http');
const numCPUs = require('node:os').availableParallelism();
const express = require('express');

if (cluster.isPrimary) {
  console.log(`Primary ${process.pid} is running`);

  // Fork workers.
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
} else {
  const app = express();
  const PORT = 3000;

 app.get('/', (req, res) => {
  res.send(`Worker ${process.pid} started``);
});
}
```


### 3. what is worker_thread in Node.js ###  
**worker_threads** is a Node.js module that allows you to run JavaScript code in parallel using multiple threads.
```javascript
const { Worker } = require("worker_threads");
const worker = new Worker("./worker.js");

worker.postMessage(10);

worker.on("message", (result) => {
    console.log("Result:", result);
});

worker.on("error", (err) => {
    console.log(err);
});
```

### 4. what is process.nextTick() in Node.js ####  
process.nextTick() is a Node.js method that executes a callback immediately after the current operation completes, 
before the event loop continues to the next phase.  
> It is used to defer the execution of a function without waiting for timers or I/O operations.  
```javascript
console.log("Start");

process.nextTick(() => {
  console.log("Inside nextTick");
});

console.log("End");
Start  
End  
Inside nextTick  
```
### 5. What is setImediate() ###  
**setImmediate()** is a Node.js function that **schedules a callback** to run after the current event loop cycle, during the "check" phase.
> It is commonly used to execute a function as soon as possible, but after I/O events and the current code have finished.
```javascript
console.log("Start");

setImmediate(() => {
  console.log("Inside setImmediate");
});

console.log("End");
Start
End
Inside setImmediate
```
> **setImmediate() callbacks are executed during the Check phase**.
**setImmediate() vs setTimeout(fn, 0)**
```javascript
const fs = require("fs");

fs.readFile(__filename, () => {
  setImmediate(() => {
    console.log("setImmediate");
  });

  setTimeout(() => {
    console.log("setTimeout");
  }, 0);
});
setImmediate
setTimeout
```
> process.nextTick() always has higher priority.

### 6. Diffrence between fs.readfile vs fs.createReadStream() ###   
**fs.readFile()** reads the entire file into memory before processing, making it suitable for small files.  
**fs.createReadStream()** reads the file in chunks using streams, which is more memory-efficient and ideal for large files such as videos, logs, and big datasets. 
---- Why is fs.createReadStream() better for large files?
1- It reads data in chunks.
2- Uses less memory.
3- Prevents blocking the event loop.
4- Improves application performance and scalability.
```javascript
const http = require("http");

http.createServer((req, res) => {
    res.end("Hello");
    res.end("World");
}).listen(3000);

// correct way //
http.createServer((req, res) => {
    if (req.url === "/") {
        return res.end("Home");
    }

    res.end("Not Found");
});
```

### 7. When res.end() twice in am HTTP server ###  
In Node.js, res.end() is **used to finish the HTTP response.** Once it is called, the response is sent to the client and the connection is considered complete.  

If you call res.end() a second time, Node.js **will throw an error** because the response has already been finished.

### 8. what is ripple invirement in Node.js ###  

### 8. why we use expresss.js ###  

### 9. Promisse in node js ###  

### 10. Event-driven architecture in Node.js ###

### 11. What is buffer in node js ###

### 12. what are streams in node js ###

### 13. what is the use for timer module in node js ##

### 14 What is body parser in node js ###

### 15. What is CORS in node js, why we need? ###

### 16. how can we implement architecture in node js ###

### 17. how can we file uploding in node js ###

### 18. how can handle email in node js ###

### 19. Example connect database in node js ###

### 20. How to handle Environment variable in node js ###

### 21. which package use password decrede in node js ###

### 22. Folder strecture in node js ###

### 23. what is eJs ###

### 24. What is Quearyparam and requestparam in node.js ###

### 25. what is google authritication ###

### 26 what is web soket in Node.Js ###

### 27. manage sassion in node js ###

### 28 create dadabase modal in node js ###

### 29  What is thread pool in node js ###

 ### 30. what is libuv in Node.Js ###

 ### 31. what are streams in Node js ###

 ### 32. what is process object in node js ###

 ### 33. What is REPL in node.js ###

 ### 34. Diffrence between require and import ###

 ### 35. what is FS module ###

 ### 36. callbacks, promise, async/await ###

 ### 37 How do you handle erroe in node js ###

 ### 38. which has hightest priority- process.nextTick(), promise, setImmediate(). setTimeout() ###

 ### 40, FS, http, path, os. event, crypto ###

 ### 41. what is package.json ####

 ### 42. what is NPX ###

 ### 43. how do you connect Node.js with mongoDB or SQL ? ###

 ### 44. What is a Momory leak in node js ###

