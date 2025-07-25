# Node.js Course / freeCodeCamp

## **What is Node.js?**

Node.js is an environment that runs JavaScript outside the browser. In 2009, it was built on the Chorme v8 engine. Node.js has a huge community. Thanks to Node.js, full-stack applications can be developed using only JS.

---
## What are the Differences Between Browser JS and Node.js?

| DOM | No DOM | JS features related to the DOM are not available in Node.js. Because Node.js does not run in a browser environment, it does not have a DOM. |
| --- | --- | --- |
| Window | No window | Similar to the DOM, Node.js does not have a Window object. |
|  No filesystem | Filesystem | Unlike browser JS, Node.js can access the filesystem and interact with the operating system. |
| Fragmentation | Versions | In browser applications, it is the developer's responsibility to test the compatibility of the user's browser with the application. However, this is not the case in Node.js, because Node.js works with a versioning system and there is no possibility of the application breaking due to versioning when development is completed on a version. |
| ES6 modules | Common.js | In the browser, modules are optional. In Node.js, modules are installed. |

---
## Working Methods with Node.js

There are two ways to work with Node.js. One is REPL and the other is CLI. REPL creates a simple Node.js environment on the command line and executes simple Node.js commands, it is only for very basic and testing purposes. The Node.js CLI is a Node command system and manages the program with commands entered through the terminal. 

For example, running 'node' on the terminal will open REPL, but running 'node myScript.js' will run a file with the CLI.

REPL is very similar to a browser console. No serious programs are developed in the browser console, only very basic and often testing operations. 

---
## Globals

Although there is no window object in Node.js, there are globals that can be accessed anywhere in the program. Some of these are as follows:

| __dirname | Path to the current folder |
| --- | --- |
| __filename | Current file name |
| require | Function required to use the modules |
| module | Information about the current module (file) |
| process | Provides information about the environment in which the program is running. When the program is deployed to a server for use in a real application, it provides information about the environment in which that server or service provider is running the program. |

---
## Modules

When using the Node.js CLI, a file is executed and this is how the program is activated. Does this mean that we just write all the software in one file? Of course not, since Node.js runs a file, all the software should be collected in one file, but this does not mean that all the software will be written in that file. 

Thanks to modules, we can share the code written by writing code to other files and import and use this shared code in other files.

In Node.js, each file is a module. And each module is 'encapsulated'. So data sharing is minimal. In this way, this module cannot be accessed from other files unless the developer exports it.

The main ways to export and import modules are as follows:

```jsx
// file 1:
const name = 'Ali'

module.exports = name

// file 2: 
const name = require('./file1') // While writing the file path here, always use the './' syntax.
```

There is another export method and it is called 'export as you go'.

```jsx
// file 1:
const name = 'Ali'

module.exports.name = name

// file 2: 
const name = require('./file1') // When writing the file path here, always use the './' syntax.
```

Although there are several different ways of doing this, the function provided is the same; to make data from one file available in another file.

### Note

If a function is defined in a module (file) and explicitly executed in the file, the function can be executed by requiring from other files without a special export.

For example:

```jsx
// file 1: 
function myFunction() {
	console.log(2+2)
}

myFunction()

// file 2: 
require('./file1')

// Output of file 2:
4
```

---
## Built-in Modules

There are some ready-made modules that come with Node.js. The most prominent of these are as follows: OS (Operating system), PATH, FS (Filesystem) and HTTP. 

When importing these modules, the './' syntax is not used like the modules created later. Only the module name is written. Also, these modules already come with Node.js so there is no need to download anything.

### OS Module

The OS module provides information about the operating system.

For example 

```jsx
const os = require('os')

// info about current user
cosnt user = os.userInfo()
console.log(user)

// method returns the system uptime in seconds
const uptime = os.uptime()
console.log(uptime)

// info about current os
const currentOS = {
	name: os.type(),
	release: os.release(),
	totalMem: os.totalmem(),
	freeMem: os.freemem()
}
console.log(currentOS)
```

### PATH Module

The Path module is a module for operations related to file paths.

For example

```jsx
const path = require('path')
console.log(path.sep) // Output: /

const filePath = path.join('content', 'subfolder', 'test.txt')
console.log(filePath) // Output: 'content/subfolder/test.txt'

const base = path.base(filePath)
console.log(base) // Output: 'test.txt'

const absolutePath = path.resolve(__dirname, 'content', 'subfolder', 'test.txt')
console.log(absolutePath) // Output:
```

### FS Module

The FS module is a module used to perform operations related to the file system. Thanks to this module, operations such as reading files, creating files and writing to files can be done. 

There are two working methods in this module; synchronous and asynchronous. 

Synchronous file reading (read):

```jsx
const { readFileSync } = require('fs')

const first = readFileSync('./root/folder/text.txt', 'utf8')
const second = readFileSync('./root/folder/text2.txt', 'utf8')

console.log(first + second) // Output: The combination of the text content from the first file and the second file.
```

Synchronous file write:

```jsx
const { readFileSync, writeFileSync } = require('fs')

writeFileSync('./root/folder/new-text.txt', 'Hello world!')
// If a file exists at the specified location, it will be overwritten.
// However, if no file exists at the specified location, a new file will be created there
// and the provided content will be written into it.

// Additionally, certain settings can be configured using the 'flag' option during the write operation.
// For example, the 'a' flag appends new content to the file without modifying the existing content:
writeFileSync(
  './root/folder/new-text.txt',
  'Hello world!',
  { encoding: 'utf8', flag: 'a' }
)
```

The main difference between synchronous file operations and asynchronous file operations is that synchronous operations work with a 'blocking' feature. That is, the synchronous operation does not proceed to the next code until it is complete. For example, if there is a time-consuming synchronous file operation, Node.js waits in that function until the operation completes and does not advance.

But asynchronous operations are the opposite. So it doesn't matter how long the operation takes. Because Node.js does not wait for this transaction to complete. It just executes that function line by line and continues to the next lines without waiting for the result.

For this reason, in a real application, when a synchronized file operation is performed, while one user is doing that operation, the other user cannot use the same feature until that user's operation is finished. 

Therefore, it is very important to consider which method to use according to the usage area and the situation.

Asynchronous file reading:

```jsx
const { readFile } = require('fs')

readFile('./root/folder/text.txt', 'utf8', (err, result) => {
	if (err) {
		console.log(err)
		return
	}
	// The remaining operations can be performed here.
	// However, this can lead to a "callback hell" situation.
	// Since functions that depend on `result` will need to be called within this callback,
	// it may cause deeply nested and hard-to-maintain code.
})
```

(how to get out of callback hell situation)

### HTTP Module (Login)

The HTTP module is the most commonly used module in Node.js. This module is used to create a server, control incoming requests and perform operations or return results based on these requests.

For example, a simple HTTP server can be created as follows:

```jsx
const http = require('http')

const server = http.createServer((req, res) => {
	if (req.url === '/') {
		res.end('Welcome to our home page!')
	} else if (req.url === '/about') {
		res.end('Here is our short history...')
	} else {
		res.end('<h1>There is not a page like that</h1>')
	}
})

server.listen(5000)
```

As in this example, the server created with the HTTPM module has two parameters, 'req' and 'res'. Req stands for 'request' and is a very large object that holds the data of the request from the user. Res stands for 'response' and refers to the response returned from the server to the user.

---
## NPM

NPM is a package/module repository and comes with the Node.js download. This repository allows developers to share their code/modules with people.

NPM allows us to do three things: 

1. Use our own code in other projects.
2. Use code written by others in our own project.
3. Share our code with others.

The most common NPM commands are as follows:

```jsx
// npm - global command, comes with Node.js
// npm --version

// local dependency - the package is used only within the current project
// npm i <packageName>

// global dependency - the package can be used across all projects
// npm install -g <packageName>

// In some cases, it is used for installing as an administrator for security purposes.
// You must enter the password of the currently active user session on your computer.
// sudo npm install -g <packageName>
```

  

### package.json

package.json is a file that lists and maintains information about the NPM modules in the project. It keeps a record of all packages downloaded to the project and registers these packages as dependencies.

There are three different ways to create the package.json file: 

```jsx
// 1. Manually create (not recommended)
// 2. npm init (step-by-step setup by answering questions)
// 3. npm init -y (automatic setup)
```

### Using NPM Packages

Below is an example of using an NPM package. An important point is that the module to be used must have been downloaded beforehand.

```jsx
const _ = request('lodash')

const items = [1, [2, [3, [4]]]]
const newItems = _.flattenDeep(items)

console.log(newItems) // Output: [1, 2, 3, 4]
```

### Sharing Code and the Importance of package.json

All modules downloaded with NPM are saved in the 'node_modules' folder. This folder is usually large in size, which causes problems when uploading to the version control system. 

The solution to this problem is to add the 'node_modules' folder to the '.gitignore' file and not upload it to GitHub. This way, the size of the file will not matter as it will only remain in the local environment.

However, one may ask; "If node_modules is not saved to GitHub, how will the modules used in the code work when this repo is cloned by someone else?". 

We would say that the solution to this problem is the package.json file. When the repo is cloned and the project is installed, a new 'node_modules' will be created on the cloned computer by downloading the necessary files according to the description in the package.json file and the code will run. Thus, node_modules, which has a serious size with each commit, will not cost to upload to GitHub.

In short, the package.json file is an 'installation guide' to get the project up and running.

### Dev Dependency

Some packages may be downloaded as 'dev dependency'. This means that these packages are only for the development environment and are not directly required for the application to run.

For example, the 'Nodemon' module automatically detects every change made to the node.js application and re-runs the application itself without us having to run a command like 'node app.js' every time.

However, this module is not indispensable for the project and its absence will not cause the project to stop working. Since Nodemon is usually only used in the development environment to make the developer's job easier, the developer requirement can be registered in package.json as a 'dev dependency'.

### Package Deletion

The 'npm uninstall <packageName>' command is used to delete packages.

### Global Package Download

Downloading modules globally means that we can use them in all projects, not just one project, without downloading them locally. If we are downloading a package and it will only be used in one project, it still makes more sense to use a local download.

Another important tool in this regard is 'npx'. npx allows us to call modules globally without even downloading them.

### package-lock.json

The package-lock.json file is a detailed log file that contains the version numbers of the requirements in the pacakge.json file and the version numbers of other packages used by these requirements. This file allows the project to be set up in a more stable and efficient way.

---
## Event Loop

To understand the event loop logic, it is first necessary to understand the synchronous and asynchronous logic. JavaScript is a 'single-core' ('single-threded') language. This means that it can only perform one operation at a time. For example, if there are two time-consuming functions, it cannot process both of them at the same time on its own.

Codes that run synchronously have the 'blocking' feature. Even if these codes take time to execute, the code must be completed before moving to the next line. 

The opposite is the case with asynchronous code. No matter how much time the code takes, it is thrown into the background with the event loop and JS continues with the next code while running with the Libuv API in the background. And it only executes this code when it is ready to be processed.

For example: 

```jsx
console.log(1)

setTimeout(() => {
	console.log(2)
}, 2000)

console.log(3)

// Output: 1, 3, 2
```

Here the output is like this because setTimeout is an asynchronous function. JS does not wait for asynchronous functions to complete, but hands them over to the OS or Libuv API to complete, and only processes them when it receives a response. The OS or Libuv are executors that are capable of multi-processing, as opposed to 'single-threaded'. The process of JS passing async code to other executors is called 'offloading'.

This prevents JS from executing time-consuming code while other tasks are neglected and other functions in the application freeze.

![image.png](image.png)

### Async Pattern

If blocking code is used after the server is created, this will cause a freeze on the whole server, 'for all users'.

Example:

```jsx
const hhtp = require('http')

const server = http.createServer((req, res) => {
	if (req.url === '/')} {
		res.end('Welcome!')
	}
	if (req.url === '/about')} {
		// ! BLOCKING CODE !
		for (let i = 0; i < 1000; i++) {
			for (let j = 0; j < 1000; j++) {
				console.log(j + i)
			}
		}
		res.end('Welcome!')
	}
})
```

When synchronized (blocking) code like in this example is used, when a user makes a request to the 'about' page, the requests of other users will also freeze. In other words, one user's request can affect the whole application in real time. 

To avoid this, long time-consuming tasks should always be done asynchronously.

### Callback Problem

We mentioned that time-consuming functions should be written asynchronously, but there are two ways to do other operations based on the result of these operations: callbacks and async/await syntax.

Callbacks are more difficult to work with in terms of readability and code writing. For example:

```jsx
const { readFile, writeFile }= require('fs')

readFile('path/to/file', 'utf8', (data1, err) => {
	if (err) {
		console.error(err)
	} else {
		const firstData = data1
		readFile('apth/to/second/file', 'utf8', (data2, err) => {
			if (err) {
				console.error(err)
			} else {
			const secondData = data2
				writeFile('path/to/write/file', (data1 + data2), () => {
					if (err) {
						console.log(err)
					} else {
						console.log('Program finished!')	
					}
				})
			}
		})
	}
})
```

The callback method is both harder to write and less readable. For this, 'async-await' is usually preferred, which does the same thing but is easier to write and much more readable.

### Async/await and Promises

The purpose of using async/await is to solve the serious readability problem that callbacks cause when they are linked one after the other. 

```jsx
console.log(1)

function myFunction() {
	console.log(2)
	
	await longWork()
	
	console.log(3)
}

myFunciton()
console.log(4)
```

```jsx
// Output:
1
2
4
// After the long process (await) has been completed.
3
```

In short, when async/await is used, the function works async in general and does not block other operations, but where await is used, the function works as sync within itself and does not continue the next operation within itself until await is completed. However, this is only valid for inside the function, so operations outside the function are not blocked.

Another important point is that every operation used with await must return a 'promise'. In other words, operations that do not return a promise are invalid. 

Therefore, await should always be used with operations that return a promise, as in the example below. Here is the cleanest refactor of the previous callback usage:

```jsx
const {readFile, writeFile} = require('fs/promises')

async function readAndWriteFiles() {
	try {
		const firstData = await readFile('path/to/file', 'utf8')
		const secondData = await readFile('path/to/second/file', 'utf8')
		
		await writeFile('path/to/write', firstData + secondData);
    console.log('Successfully writed to the file.');
	} catch (err) {
		console.error(err)
	}
}
```

As you can see, the second use is much cleaner and more readable than the first callback.

---
## Events

Node.js is event-driven. In other words, an event scenario is usually defined, the code to be executed when the event occurs is prepared, and the previously defined code is executed when the event is triggered by listening to the event.

```jsx
const EventEmitter = require('events')

const customEmitter = new EventEmitter()

customEmitter.on('request', () => {
	console.log('Event triggered!')
})

customEmitter.emit('request')

// Usage with parameters:

customEmitter.on('request', (name, age) => {
	console.log('Event triggered by:', name, age)
})

customEmitter.emit('request', 'Ahmet', 30)
```

A more realistic use case:

```jsx
const http = require('http')

const server = http.createServer()

server.on('request', (req, res) => {
	res.end('Welcome!')
})

server.listen(5000)
```

---
## Streams

In Node.js, streams have a very important place in terms of data operations. The purpose of streams is to read, write and manipulate data in an 'optimized and efficient' way. 

Streams are not really needed for small data operations. The main use of streams is for big data. Because small data of a few bytes does not cause performance problems.

But with big data the situation is different. RAM is used when reading or writing data, and sending GBs of data to RAM as a whole can cause serious performance losses and even crash Node.js. 

For example, if we try to read 5GB of data with 'readFile', this data will be sent to RAM in 'one piece' and this can cause huge performance losses.

However, when reading data with streams, this 5GB file will be sent to RAM in 'chunks', which are 64 bytes by default, and will be processed 'piece by piece' to prevent RAM bloat. 

Example:

```jsx
const {createReadStream} = require('fs')

const stream = createReadStream('path/long_text.txt')

stream.on('data', (chunk) => {
	console.log(chunk)
})
```

In the above example, a stream is first created with 'creatReadStream' and the contents of the file at the specified location are sent to RAM in chunks by this asynchronous process. From there, after each chunk is transferred to RAM, the specified event listener is used to take the specified action for the chunk. In this example, console.log is processed.

That is, the stream sends the file to RAM piece by piece and the event listener takes the incoming chunks one by one and performs the action.

If no action is specified, the chunks still do not accumulate in RAM and are deleted after a few chunks. This avoids performance problems and allows data operations to be performed efficiently.

There are four stream types:

| Stream Type | Description | Example Usage |
| --- | --- | --- |
| **Readable** | Stream that can only read data | `fs.createReadStream()` (read file) |
| **Writable** | Stream that can only write data | `fs.createWriteStream()` (write file) |
| **Duplex** | Stream that can both read and write | TCP sockets, WebSocket connection |
| **Transform** | Dedicated Duplex stream that converts incoming data | `zlib.createGzip()` (compression), encryption |

Here are a few examples of streams:

```jsx
const fs = require('fs');

const stream = fs.createReadStream('file.txt');

stream.on('data', (chunk) => {
  console.log('Chunk:', chunk.length);
});

stream.on('end', () => {
  console.log('Finsihed reading file');
});
```

```jsx
// Importing a large JSON file into MongoDB in chunks.
const fs = require('fs');
const { MongoClient } = require('mongodb');

const client = new MongoClient('mongodb://localhost:27017');
client.connect().then(() => {
  const db = client.db('test');
  const collection = db.collection('data');

  const stream = fs.createReadStream('large.json', { encoding: 'utf8' });
  let buffer = '';

  stream.on('data', (chunk) => {
    buffer += chunk;
    let sınır;
    while ((sınır = buffer.indexOf('\n')) >= 0) {
      const satır = buffer.slice(0, sınır);
      buffer = buffer.slice(sınır + 1);
      const json = JSON.parse(satır);
      collection.insertOne(json);
    }
  });
});
```

```jsx
// While reading the log file, compress and write it to a new .gz file.
const fs = require('fs');
const zlib = require('zlib');

const readStream = fs.createReadStream('log.txt');
const writeStream = fs.createWriteStream('log.txt.gz');
const gzip = zlib.createGzip();

readStream.pipe(gzip).pipe(writeStream);
```

```jsx
// The server sends the file or another resource over the network in chunks according to the incoming request.
const http = require('http');
const fs = require('fs');

http.createServer((req, res) => {
  const stream = fs.createReadStream('film.mp4');
  res.writeHead(200, { 'Content-Type': 'video/mp4' });
  stream.pipe(res);
}).listen(3000);
```

---
## HTTP Messages

Communication between the user device (client) and the server (server) is achieved through HTTP messages. These messages are of two types; 'request' messages sent by the client and 'response' messages sent by the server. 

### Request Messages

Request messages are the client's way of communicating its request to the server. This message, which is sent to the destination internet address (url) over the HTTP protocol, is received by the server and necessary actions are taken according to the content of the message. This message has a structure consisting of several parts. These parts are as follows:

1. **Request line:**
    
    The request line contains basic information, including the type of message, where it is directed and the HTTP version used.
    
2. **Headers:**
    
    Headers carry the metadata of the message and contain information such as data type, authorization, content length. They are usually generated automatically, but in some cases they can also be set manually.
    
3. **Body - Optional:**
    
    If the message will carry data to the server, this data will be included in the body. Although this part is optional, its frequency of use may vary depending on the message type. For example, while it is not used much in GET messages, it is used more frequently in POST messages. The data in the body can be of different types, but generally JSON type data is transferred.
    

There are 4 common methods that must be included in the 'request line', which is mandatory in request messages and indicates the request type of the message; GET, POST, PUT, DELETE.

| GET | Used to retrieve data. |
| --- | --- |
| POST | Used to send data. |
| PUT | Used to update existing data. |
| DELETE | Used to delete existing data. |

A sample HTTP Request message:

```jsx
// Request line:
GET /www.news.com HTTP/1.1

// Headers:
Host: example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOi...

// Body:
{
  "username": "user312",
  "age": 41
}
```

### Response Messages

Response messages are messages sent in response to request messages sent by the client. For example, when the client clicks on a website link in the browser, a request is sent to the server to get the HTML code of the website, the server prepares this code and sends it as a response message.

Response messages consist of 3 main parts just like request messages:

1. **Status line:**
    
    The status line contains the HTTP version, status code and status message. The status code is a code that expresses status information, such as whether the requested operation has completed or not, and the status text is a textual description of it.
    
2. **Headers:**
    
    Contains metadata like in request messages.
    
3. **Body:**
    
    As in request messages, it expresses the content of the message, it is the section containing the data to be sent and can be present or absent depending on the operation performed.
    

A sample response message: 

```jsx
// Status line
HTTP/1.1 200 OK

// Headers
Content-Type: application/json
Content-Length: 54
Connection: keep-alive

// Body
{
  "message": "POST request received successfully.",
  "success": true
}
```

---
## Simple HTTP Server Example

The following is a simple HTTP server using request and response objects and various methods.

```jsx
const http = require('http')

const server = http.createServer((req, res) => {
	const url = req.url
	
	// home page
	if (url === '/') {
		res.writeHead(200, {'content-type' : 'text/html'})  // writeHeader -> writeHead olarak düzeltildi
		res.write('<h1>Welcome to our site!</h1>')
		res.end()
	}
	
	// about page
	else if (url === '/hakkinda') {
		res.writeHead(200, {'content-type' : 'text/html'})
		res.write('<h1>This is an about page</h1>')
		res.end()
	}
	
	// contact page
	else if (url === '/iletisim') {
		res.writeHead(200, {'content-type' : 'text/html'})
		res.write('<h1>This is a contact page</h1>')
		res.end()
	}
	
	// 404 page
	else {
		res.writeHead(404, {'content-type' : 'text/html'})
		res.write('<h1>The page you are looking for was not found</h1>')
		res.end()
	}
})

server.listen('5000', () => {
	console.log('User has connected to the server.')
})
```

The main methods used in this example are writeHeader, write and end methods. When using the HTTP module, operations such as adding a header to the response, adding a body to the response are done through these methods, while when working in Express, the same functions are performed in different ways.

The**writeHeader()** method is used to add a header to the request message received by the client while creating a new response message. Thus, information such as the status and content type of the sent message can be easily accessed.

The**write()** method is used to add body to the response message. The content-type in the header created with writeHeader directly affects how this body section will be read in the browser. For example, if the content-type 'text/plain' in the header is '<h1>Hello world!</h1>', it will appear as '<h1>Hello world!</h1>' directly on the browser screen. However, if the content-type in the head is 'text/html', the browser will display 'Hello world!' in the header style because the browser recognizes the type of content according to the header and acts accordingly. 

The**end()** method completes the response object created with the previous methods and sends it to the browser.

### Sending a File as a Response

In real applications, a file is almost always added to the body field with the write() method. This file can be of different types such as .html, .css, .js, .ts, .xml, etc. This is done as follows:

```jsx
const http = require('http')
const {readFileSync} = require('fs')

const anasayfa = readFileSync('./index.html')

const server = http.createServer((req, res) => {
	const url = req.url
	
	if (url === '/') {
		res.writeHeader(200, {'content-type': 'text/html'})
		res.write(anasayfa)
		res.end()
	}
})
```

Thus, all the content in the index.html file will be read by the browser and delivered to the user according to the content type specified in the header.

However, there is an important point here. That is the CSS or JS files that this index.html file needs within itself. These files are returned to the server as a GET type request message while reading index.html. For these requests, it is necessary to read all the files manually on servers using the classic HTTP module and set the sending process of these files according to the request url.

For example:

```jsx
const http = require('http')
const {readFileSync} = require('fs')

const anasayfa = readFileSync('./index.html')
const style = readFileSync('.style.css')
const javascript = readFileSync('index.js')

const server = http.createServer((req, res) => {
	const url = req.url
	
	if (url === '/') {
		res.writeHeader(200, {'content-type': 'text/html'})
		res.write(anasayfa)
		res.end()
	}
	
	if (req === '/style.css') {
		res.writeHeader(200, {'content-type': 'text/css'})
		res.write(style)
		res.end()
	}
	
	if (req === '/index.js') {
		res.writeHeader(200, {'content-type': 'text/javascript'})
		res.write(style)
		res.end()
	}
})
```

However, this process is inefficient and unsustainable for projects that contain tens or hundreds of files. The solution to this problem is Express.js. 

---
## Express.js

Express.js is a Node.js library (framework). Thanks to this framework, many operations that are complicated to do in the original (vanilla) Node.js, such as sending files, can be done much more easily. 

Since Express.js is a framework, it is not automatically installed in Node.js installation. For this reason, Express.js, which is an external module, can only be used after downloading it with 'npm'.

A simple Express.js server has been created below:

```jsx
const express = require('express')
const app = express()

app.get('/', (req, res) => {
	console.log('User logged in.')
	res.status(200).send('Home Page')
})

app.get('/about', (req, res) => {
	res.status(200).send('About Us Page')
})

app.all('*', (req, res) => {
	res.status(404).send('<h1>The page you are looking for cannot be found!</h1>')
})

app.listen('5000', () => {
	console.log('Listening on port 5000.')
})
```

Below are the most commonly used methods with servers built with Express: 

```jsx
// app.get: Handles GET requests coming from the client.
// app.post: Handles POST requests coming from the client.
// app.delete: Handles DELETE requests coming from the client.
// app.all: Handles all types of requests coming from the client.
// app.use: Used to create middleware.
// app.listen: Continuously listens to the specified domain or port.
```

### Serving Static Files with Express.js

The necessity of having to do multiple file serving manually, which we previously mentioned in vanilla Node.js, can be easily solved with Express.js.  

With the express.static method, all files in a specified location are recognized as static and when a request is made to the server for these files, the requested file is automatically sent to the client. Thus, the developer does not need to import each file one by one and write separate 'if' blocks for all these files.

Example:

  

```jsx
const express = require('express')  // 'exporess' → 'express' olarak düzeltildi
const path = require('path')         // {path} değil, doğrudan require('path') olmalı
const app = express()

// By creating middleware, the locations of all files in the specified folder were determined.
// These files are saved to be sent when requested.

app.use(express.static('./public'))

app.get('/', (req, res) => {
	res.sendFile(path.resolve(__dirname, './ornek/dosya.html'))  // Türkçe karakterler kaldırıldı (örnek → ornek)
})

app.all('*', (req, res) => {
	res.status(404).send('The page you are looking for was not found.')  // res.statu → res.status olarak düzeltildi ve İngilizce çeviri yapıldı
})

app.listen('5000', () => {
	console.log('Listening on port 5000.')
})
```

---
## Route Params

Route Params is a structure that transfers data to the server with the requested URL and receives a response according to this data.

For example, let's imagine that there is a route where products are shown on an e-commerce site. When a request is sent to the backend with this route, all products will be received as a response. However, when the user wants to click on a product and go to the page of that product, it is mandatory to specify this product specifically in the request sent to the server. 'route params' can be used to do this.

Example:

```jsx
// Showcase page URL: app/products/

app.get('app/products/', (req, res) => {
	res.status(200).send(products)
})

// Specific product URL: app/products/:productId
// A colon ':' is placed before the query string parameter to indicate it is variable.

app.get('app/products/:productId', (req, res) => {
	const { productId } = req.params
	
	const requestedProduct = products.find(
    (product) => product.id === Number(productId)
  )
  if (!requestedProduct) {
    return res.status(404).send('Product Does Not Exist')
  }

  return res.json(requestedProduct)
	
	// Note: This line won't be reached because of the return above
	// res.status(200).send(responseProducts)
})
```

### Using Return to Send Response in If Blocks

In cases where a response is sent in If blocks, if there is a response outside the block, it is mandatory to use the return keyword before the response in the block.

The reason for this is that when return is not used, Node.js continues to read the code after the response in the block is sent and executes the response code outside the block. And only one response can be sent to a request within the same app method (e.g. app.get, app.post). Since res.send does not terminate the app method, in cases where there is a possibility of multiple responses, return must be used to avoid getting an error.

Example:

```jsx
const express = require('express')
const app = express()

app.get('app/products/:productId', (req, res) => {
	const { productId } = req.params
	
	const requestedProduct = products.find(
    (product) => product.id === Number(productId)  // productID → productId olarak düzeltildi
  )
  if (!requestedProduct) {
    // If 'return' is not used here, the next response will also be sent, causing an error.
    // The 'return' keyword ensures exiting the active app.get callback. However, res does not do this.
    return res.status(404).send('Product Does Not Exist')
  }

  return res.json(requestedProduct)
	
	// This line will not be reached because of the return above
	// res.status(200).send(responseProducts)
})

app.listen('5000')

```

---
## Query String

Route params can be used to query a specific page as a simple query or more advanced queries can be made with 'query strings'. With the same logic, some query parameters are sent with the URL and the server filters the data with these parameters and sends the desired answer.

To add query strings to the URL, the '?' sign is used. After the '?' sign, it is considered a query string. More than one query string can be specified by adding '&' between them. Variables to be used as query strings must be registered in the backend.

For example:

```jsx
app/products/?search=telefon&limit=20
```

Otherwise they are not valid.

General query string usage example:

```jsx
const express = require('express')
const app = express()
const { products } = require('./data')

app.get('/', (req, res) => {
  res.send('<h1> Home Page</h1><a href="/api/products">products</a>')
})

app.get('/api/v1/query', (req, res) => {
  // console.log(req.query)
  const { search, limit } = req.query
  let sortedProducts = [...products]

  if (search) {
    sortedProducts = sortedProducts.filter((product) => {
      return product.name.startsWith(search)
    })
  }
  if (limit) {
    sortedProducts = sortedProducts.slice(0, Number(limit))
  }
  if (sortedProducts.length < 1) {
    // res.status(200).send('no products matched your search');
    return res.status(200).json({ sucess: true, data: [] })
  }
  res.status(200).json(sortedProducts)
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

---
## Middleware

Middleware is the name given to the processing layer between request messages and response messages. When a request comes to the server, certain operations are performed in this layer before sending a response message and then the response message is sent.

Middleware can be used for many critical operations such as reporting (logging), security control (authentication).

In addition, the result of the operation in the middleware phase can also be reflected in the response phase. For example, if the user authentication phase fails, an error message can be returned in the response.

A simple middleware structure is as follows:

```jsx
const express = require('express')
const app = express()

//  req => middleware => res

const logger = (req, res, next) => {
  const method = req.method
  const url = req.url
  const time = new Date().getFullYear()
  console.log(method, url, time)
  
  next() // next() allows moving to the next step and is mandatory. 
	// If not used, the client will enter a loading loop.

app.get('/', logger, (req, res) => {
  res.send('Home')
})

app.get('/about', logger, (req, res) => {
  res.send('About')
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

However, the weakness of this structure is that middleware functions have to be called directly 'manually for each request separately'. For example, it is difficult to manually add middleware to each method for dozens of different requests. For this, there is an '**app.use**' method that automatically adds the specified middleware to the specified routes.

The optimized usage is as follows:

```jsx
const express = require('express')
const app = express()
const logger = require('./logger')
const authorize = require('./authorize')

//  req => middleware => res
app.use('/', [logger, authorize]) // When no route is specified, the middleware is applied to all routes.
	
app.get('/', (req, res) => {
  res.send('Home')
})

app.get('/about', (req, res) => {
  res.send('About')
})

app.get('/api/products', (req, res) => {
  res.send('Products')
})

app.get('/api/items', (req, res) => {
  console.log(req.user)
  res.send('Items')
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

As the scope of applications grows, the type of middleware needed also grows. One way to meet this need is to write middleware functions from scratch, or to use Express.js's own middleware and download middleware from NPM.

Example

```jsx
app.use([logger, authorize]) // They can be created manually.
app.use(express.static('./public')) // Provided by Express to define file paths.
app.use(morgan('tiny')) // Installed from NPM for logging.
```

---
## Example Method Uses

Below is a simple example of the different request methods:

```jsx
const express = require('express')
const app = express()
let { people } = require('./data')

// static assets
app.use(express.static('./methods-public'))
// parse form data
app.use(express.urlencoded({ extended: false }))
// parse json
app.use(express.json())

app.get('/api/people', (req, res) => {
  res.status(200).json({ success: true, data: people })
})

app.post('/api/people', (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).json({ success: true, person: name })
})

app.post('/api/postman/people', (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).json({ success: true, data: [...people, name] })
})

app.post('/login', (req, res) => {
  const { name } = req.body
  if (name) {
    return res.status(200).send(`Welcome ${name}`)
  }

  res.status(401).send('Please Provide Credentials')
})

app.put('/api/people/:id', (req, res) => {
  const { id } = req.params
  const { name } = req.body

  const person = people.find((person) => person.id === Number(id))

  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${id}` })
  }
  const newPeople = people.map((person) => {
    if (person.id === Number(id)) {
      person.name = name
    }
    return person
  })
  res.status(200).json({ success: true, data: newPeople })
})

app.delete('/api/people/:id', (req, res) => {
  const person = people.find((person) => person.id === Number(req.params.id))
  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${req.params.id}` })
  }
  const newPeople = people.filter(
    (person) => person.id !== Number(req.params.id)
  )
  return res.status(200).json({ success: true, data: newPeople })
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

---
## Router Structure

As in classic Express.js applications, performing all requests directly on the app object causes code clutter and reduces readability. To solve this problem, we can create a file for each different route and use a custom Router structure.

For example:

```jsx
// Main file: 
const express = require('express')
const app = express()

const people = require('./routes/people')
const auth = require('./routes/auth')

// static assets
app.use(express.static('./methods-public'))
// parse form data
app.use(express.urlencoded({ extended: false }))
// parse json
app.use(express.json())

app.use('/api/people', people)
app.use('/login', auth)

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})

// people.js:
const express = require('express')
const router = express.Router()

const {
  getPeople,
  createPerson,
  createPersonPostman,
  updatePerson,
  deletePerson,
} = require('../controllers/people')

// First usage method:
// router.get('/', getPeople)
// router.post('/', createPerson)
// router.post('/postman', createPersonPostman)
// router.put('/:id', updatePerson)
// router.delete('/:id', deletePerson)

// Second usage method:
router.route('/').get(getPeople).post(createPerson)
router.route('/postman').post(createPersonPostman)
router.route('/:id').put(updatePerson).delete(deletePerson)

module.exports = router

// auth.js:
const express = require('express')
const router = express.Router()

router.post('/', (req, res) => {
  const { name } = req.body
  if (name) {
    return res.status(200).send(`Welcome ${name}`)
  }

  res.status(401).send('Please Provide Credentials')
})

module.exports = router
```

This makes the main file much cleaner and more readable.

### Controllers in the router structure

It is also important to use controllers to further improve readability. The logic of using controllers is to use the callback function, which is the second parameter in the router.get('/') method, by creating it as a function in another file and passing it to the router file as a name only.

For example:

```jsx
// Controllers/people.js:
let { people } = require('../data')

const getPeople = (req, res) => {
  res.status(200).json({ success: true, data: people })
}

const createPerson = (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).send({ success: true, person: name })
}

const createPersonPostman = (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).send({ success: true, data: [...people, name] })
}

const updatePerson = (req, res) => {
  const { id } = req.params
  const { name } = req.body

  const person = people.find((person) => person.id === Number(id))

  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${id}` })
  }
  const newPeople = people.map((person) => {
    if (person.id === Number(id)) {
      person.name = name
    }
    return person
  })
  res.status(200).json({ success: true, data: newPeople })
}

const deletePerson = (req, res) => {
  const person = people.find((person) => person.id === Number(req.params.id))
  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${req.params.id}` })
  }
  const newPeople = people.filter(
    (person) => person.id !== Number(req.params.id)
  )
  return res.status(200).json({ success: true, data: newPeople })
}

module.exports = {
  getPeople,
  createPerson,
  createPersonPostman,
  updatePerson,
  deletePerson,
}
```
