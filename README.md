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
REPL stands for **Read, Evaluate, Print**, and **Loop**. It is an interactive shell provided by Node.js that allows developers to execute JavaScript code line by line and immediately see the results. It is mainly used for testing, debugging, and learning.

### 8. why we use expresss.js ###  
**Express.js** is a fast, lightweight, and flexible web framework for Node.js that helps developers build web applications and REST APIs easily.  
Example: Without Express.js (this becomes difficult to manage.)  
```javascript
const http = require("http");

const server = http.createServer((req, res) => {
    if (req.url === "/") {
        res.end("Home Page");
    } else if (req.url === "/about") {
        res.end("About Page");
    }
});

server.listen(3000);
```
Example: >With Express.js  (The code is shorter and easier to maintain.)   
```javascript
const express = require("express");

const app = express();

app.get("/", (req, res) => {
    res.send("Home Page");
});

app.get("/about", (req, res) => {
    res.send("About Page");
});

app.listen(3000, () => {
    console.log("Server is running on port 3000");
});
```

### 9. Promisse in node js ###  
A Promise is an object that represents the eventual completion or failure of an asynchronous operation. It has three states: **Pending, Fulfilled, and Rejected.** Promises use .then(), .catch(), and .finally() to handle results and are widely used with async/await  
>A Promise in Node.js is an object used to handle asynchronous operations, representing a value that may be available now, later, or never.
```javascript
| Method                 | Description                             |  
| ---------------------- | --------------------------------------- |  
| `Promise.resolve()`    | Creates a resolved Promise              |  
| `Promise.reject()`     | Creates a rejected Promise              |  
| `Promise.all()`        | Waits for all Promises                  |  
| `Promise.race()`       | Returns the first completed Promise     |  
| `Promise.allSettled()` | Returns all results, success or failure |  
| `Promise.any()`        | Returns the first successful Promise    |
```


### 10. Event-driven architecture in Node.js ###
Event-driven architecture in Node.js is a design pattern where events trigger callbacks, allowing asynchronous and non-blocking execution of code  
```javascript
| Method                 | Description                |
| ---------------------- | -------------------------- |
| `on()`                 | Register an event listener |
| `emit()`               | Trigger an event           |
| `once()`               | Execute listener only once |
| `off()`                | Remove an event listener   |
| `removeAllListeners()` | Remove all listeners       |

```
**Why do we use Event-Driven Architecture?**  

✅ Non-blocking I/O  

✅ High performance  

✅ Handles thousands of concurrent requests  

✅ Efficient resource utilization  

✅ Perfect for real-time applications  

### 11. What is buffer in node js ###  
A Buffer is a global object in Node.js used to store and manipulate **binary data**.  
Since JavaScript does not handle binary data directly, Node.js provides the Buffer class.
**Why do we use Buffer?**  
Suppose you read an image file.
The image is not stored as text; it is stored as binary data (0s and 1s). Node.js uses Buffers to handle this binary data efficiently.
**Example:** Creating a Buffer  
```javascript
const buffer = Buffer.from("Hello");
console.log(buffer); //output <Buffer 48 65 6c 6c 6f> //

//Convert Buffer to String //
const buffer = Buffer.from("Hello");
console.log(buffer.toString()); // output Hello ///
```
**Example:** Buffer Example with File System  
```javascript
// sample.txt //  "Hello Node.js"
const fs = require("fs");

fs.readFile("sample.txt", (err, data) => {
    if (err) throw err;

    console.log(data);            // Buffer
    console.log(data.toString()); // Text
});
// output <Buffer 48 65 6c 6c 6f 20 4e 6f 64 65 2e 6a 73> //
// output Hello Node.js //

| Method              | Description                |
| ------------------- | -------------------------- |
| `Buffer.from()`     | Create a buffer from data  |
| `Buffer.alloc()`    | Create a fixed-size buffer |
| `buffer.toString()` | Convert buffer to string   |
| `buffer.write()`    | Write data into a buffer   |
| `buffer.length`     | Get buffer size            |

```

### 12. what are streams in node js ###  
A Stream in Node.js is a way to **read or write data piece by piece (chunks)** instead of loading the entire data into memory.  
Streams are used to handle **large files**  

**Why do we use Streams?**  
Suppose you have a 2 GB video file.
**Without Streams** (fs.readFile())  
```javascripr
2 GB File ---> Load Entire File into Memory ---> Process
// High memory usage. //
```
**With Streams:**  
```javascript
2 GB File
  Chunk 1 ---> Process
  Chunk 2 ---> Process
  Chunk 3 ---> Process
```
**Readable Stream** - reading a file  
```javascript
const fs = require("fs");

const stream = fs.createReadStream("sample.txt", "utf8");

stream.on("data", (chunk) => {
    console.log(chunk);
});
```
**Writable Stream**  
```javascript
const fs = require("fs");

const stream = fs.createWriteStream("output.txt");

stream.write("Hello ");
stream.write("Node.js");
stream.end();
```
**Duplex Stream** A Duplex stream can both read and write.  
```javascript
Read <------> Write
```
**Transform Stream** 
A Transform stream can change the data while passing it through.  
Example: Compressing a file using zlib.  
```javascript
const fs = require("fs");
const zlib = require("zlib");

fs.createReadStream("input.txt")
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream("input.txt.gz"));
```
**Example:**  
```javascript
const fs = require("fs");

const stream = fs.createReadStream("sample.txt");

stream.on("data", (chunk) => {
    console.log(chunk.toString());
});

stream.on("end", () => {
    console.log("Reading completed");
});
```


### 13. What is the use for timer module in node js ###  
The **Timer module** in Node.js - helps perform tasks asynchronously without blocking the event loop.  
**Common Timer Functions**  
```javascript
setTimeout()	   Executes a function once after a specified delay
setInterval()	   Executes a function repeatedly after a specified interval
setImmediate()	 Executes a function after the current event loop cycle
clearTimeout()	 Cancels a setTimeout()
clearInterval()	 Cancels a setInterval()
clearImmediate() Cancels a setImmediate()

// **setInterval**
let count = 0;
const timer = setInterval(() => {
    count++;
    console.log(count);

    if (count === 5) {
        clearInterval(timer);
    }
}, 1000);

// setImmediate -  Executes a callback during the check phase of the event loop. //
console.log("Start");

setImmediate(() => {
    console.log("Immediate");
});

console.log("End");
```

### 14 What is body parser in node js ###
Body Parser is Express middleware that **parses incoming request bodies** and makes the data available in **req.body**. It is commonly used to handle JSON and form data sent by clients.
>Body Parser is middleware used in Express.js to read and parse the data sent in the HTTP request body.  
>When a client sends data using a POST, PUT, or PATCH request, the data is available in the request body. Body Parser converts that data into a JavaScript object so that you can access it using req.body.

Why do we use Body Parser?   
```javascript
/// client send the data //
{
  "name": "Ajay",
  "age": 25
}

// Without body parser, req.body will be: //
output:  undefined

With body parser: req.body
output
{
  name: "Ajay",
  age: 25
}
```
**Example:**  
```javascript
const express = require("express");
const bodyParser = require("body-parser");

const app = express();

app.use(bodyParser.json());

app.post("/user", (req, res) => {
    console.log(req.body);
    res.send("Data received");
});

app.listen(3000);

URL-Encoded Parser  
app.use(express.urlencoded({ extended: true }));
```
Step 2: Use the middleware
```javascript
const express = require("express");
const cors = require("cors");

const app = express();

app.use(cors());

app.get("/", (req, res) => {
    res.send("CORS Enabled");
});

app.listen(5000);
```
Step 3: Allow Only Specific Origin
```javascript
const cors = require("cors");
app.use(cors({
    origin: "http://localhost:3000"
}));
```


### 15. What is CORS in node js, why we need? ###
**CORS** stands for **Cross-Origin Resource Sharing**.  
It is a **browser security feature** that controls whether a web page can request resources from a different origin (domain, protocol, or port).  
it is  allows or restricts web applications from making requests to a different origin.  
**What is an Origin?**  
- Protocol (http or https)  
- Domain (example.com)  
- Port (3000, 5000, etc.)  
Example
```javascript
Frontend: http://localhost:3000

Backend API: http://localhost:5000  
```
The abaove ports are different, these are different origins.  

> CORS allows the server to specify which origins are allowed to access its resources.
```javascript
ERROR:
Access to fetch at 'http://localhost:5000'
from origin 'http://localhost:3000'
has been blocked by CORS policy.
```
**Common CORS Headers**  
```javascript
Header	Purpose
Access-Control-Allow-Origin	       Allowed origin
Access-Control-Allow-Methods	     Allowed HTTP methods
Access-Control-Allow-Headers	     Allowed request headers
Access-Control-Allow-Credentials	 Allows cookies/authentication
example:
Access-Control-Allow-Origin: http://localhost:3000
```
How do you enable CORS in Express?  
```javascript
const cors = require("cors");
app.use(cors());

//Or for a specific origin //
app.use(cors({
    origin: "http://localhost:3000"
}));
```

### 16. how can we implement architecture in node js ###  
The most common architecture for Node.js applications is **Layered Architecture (MVC)** or **Clean Architecture**.  
Step-1: **MVC (Model-View-Controller) Architecture**  
```javascript
Client Request >  Routes >  Controller > Service (Optional) > Model > Database
```
Folder structureL  
```javascript
project/
│
├── controllers/
│     └── userController.js
│
├── models/
│     └── userModel.js
│
├── routes/
│     └── userRoutes.js
│
├── services/
│     └── userService.js
│
├── middleware/
│
├── config/
│
├── app.js
└── server.js
```
**Route:**   
```javascript
const express = require("express");
const router = express.Router();
const userController = require("../controllers/userController");

router.get("/users", userController.getUsers);

module.exports = router;
```
**Controller:**   
```javascript
const userService = require("../services/userService");

exports.getUsers = async (req, res) => {
    const users = await userService.getAllUsers();
    res.json(users);
};
```
**Service**   
```javascript
const userModel = require("../models/userModel");

exports.getAllUsers = () => {
    return userModel.find();
};
```
Model  
```javascript
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
    name: String
});

module.exports = mongoose.model("User", userSchema);
```
**Configuration**   
```javascript
config/
    database.js
    env.js

// Store: //
Database URLs
JWT secrets
Environment variables
```
**Error Handling**  
```javascript
app.use((err, req, res, next) => {
    res.status(500).json({
        message: err.message
    });
});
```
Why do we use the Service Layer?  
The Service Layer contains the business logic, keeping controllers lightweight and making the code reusable and easier to test.  

### 17. how can we file uploding in node js ###  
In Node.js, file uploading is commonly using:  
Express.js  and  Multer middleware  
**Multer** handles **multipart/form-data**, which is used for uploading files.

Example : 
```javascript
//Create Express Server//
const express = require("express");
const multer = require("multer");

const app = express();

// Configure Storage//
const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, "uploads/");
    },
    filename: (req, file, cb) => {
        cb(null, Date.now() + "-" + file.originalname);
    }
});

const upload = multer({ storage: storage });

// Create Upload API //
app.post("/upload", upload.single("file"), (req, res) => {
    res.send("File uploaded successfully");
});

// Start the Server // 
app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```
**HTML Form Example** //  
```javascript
<form action="/upload" method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <button type="submit">Upload</button>
</form>
```
Upload Multiple Files  
```javascript
app.post("/upload", upload.array("files", 5), (req, res) => {
    res.send("Multiple files uploaded");
});
// This allows uploading up to 5 files.
```
Access Uploaded File Details  
```javascript
app.post("/upload", upload.single("file"), (req, res) => {
    console.log(req.file);

    res.json(req.file);
});


{
    "fieldname": "file",
    "originalname": "photo.jpg",
    "filename": "1750000000-photo.jpg",
    "destination": "uploads/",
    "size": 102400
}
```
Restrict File Types  
```javascript
const upload = multer({
    storage: storage,
    fileFilter: (req, file, cb) => {
        if (
            file.mimetype === "image/jpeg" ||
            file.mimetype === "image/png"
        ) {
            cb(null, true);
        } else {
            cb(new Error("Only JPG and PNG files are allowed"));
        }
    }
});
```
Limit file size  
```javascript
const upload = multer({
    storage: storage,
    limits: {
        fileSize: 2 * 1024 * 1024
    }
});
```

### 18. how can handle email in node js ###  
In Node.js, emails are commonly sent using the Nodemailer package.  

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

