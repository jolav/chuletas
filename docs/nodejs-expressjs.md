# NODEJS 12.X.X + EXPRESS 4.16.X

---

## NPM

`npm install paquete -g` - Install package globally. Global packages are usually for executable commands.  
`npm install paquete` - Install package locally. Local packages are for the use of require in the app.   
`npm uninstall paquete -g` - Uninstall global package.   
`npm uninstall paquete` - Uninstall local package.  
`npm install package@0.0.0`- Install specific version  

`npm list -g --depth=0` - List global packages    
`npm list --depth=0` - List local packages   

`npm view paquete version` -   
`npm search package` -   

`npm update -g` - Update global packages   
`npm update` - Update local packages  

`npm outdated -g` - list outdated global libraries  
`npm outdated` - list outdated local libraries  

`npm -v` - show installed npm version  

`npm install -g npm-check-updates`  
`ncu -g` - check global packages   
`ncu -g -u` - upgrade globak packages  
`ncu` - check local package.json  
`ncu -u` - upgrade local package.json    

---

## CORE MODULES

### fs (file system)

[File System](https://nodejs.org/dist/latest-v8.x/docs/api/fs.html)

`fs.appendFile(file, data[, options], callback)` - Asynchronously append data to a file, creating the file if it does not yet exist.  
`fs.appendFileSync(file, data[, options])` - Synchronously append data to a file, creating the file if it does not yet exist.  
`fs.close(fd, callback)` - close file   
`fs.closeSync(fd)` - sync version of close file  
`fs.createReadStream(path[, options])` - Returns a new ReadStream object    
`fs.createWriteStream(path[, options])` - Returns a new WriteStream object    
`fs.mkdir(path[, mode], callback)` - create folder    
`fs.mkdirSync(path[, mode])` - sync version of create folder    
`fs.open(path, flags[, mode], callback)` - open file  
`fs.openSync(path, flags[, mode])` - sync version of open file    
`fs.readdir(path, callback(error, files))` - Reads the contents of a directory.   
`fs.readdirSync(path)` - Returns an array of filenames excluding '.' and '..'    
`fs.readFile(file[, options], callback(err, data))` - Asynchronously reads the entire contents of a file  
`fs.readFileSync(file[, options])` - Synchronous version of fs.readFile()  
`fs.read(fd, buffer, offset, length, position, callback)` - Read data from the file  
`fs.readSync(fd, buffer, offset, length, position)` - Sync version of Read data from the file  
`fs.rename(oldPath, newPath, callback)` - change the name or location of a file  
`fs.renameSync(oldPath, newPath)` - Sync version of change the name or location of a file  
`fs.rmdir(path, callback)` - delete a directory   
`fs.rmdirSync(path)` - Sync version of delete a directory    
`fs.stat(path, callback(err, stats))` - get file status  
`fs.statSync(path)` -  sync get file status    
`fs.unlink(path, callback)` - delete file   
`fs.unlinkSync(path)` - Sync version of delete file  
`fs.unwatchFile(filename[, listener])` - Stop watching for changes on filename  
`fs.watch(filename[, options][, listener])` - Watch for changes on filename, where filename is either a file or a directory.  
`fs.watchFile(filename[, options], listener)` - Watch for changes on filename, where filename is a file.  
`fs.write(fd, buffer, offset, length[, position], callback)` - Write buffer to the file  
`fs.writeSync(fd, buffer, offset, length[, position])` - Sync Write buffer to the file  
`fs.write(fd, string[, position[, encoding]], callback)` - Write string to the file  
`fs.writeSync(fd, string[, position[, encoding]])` - Sync Write string to the file     
`fs.writeFile(file, data[, options], callback)` - writes data to a file, replacing the file if it already exists. data can be a string or a buffer.    
`fs.writeFileSync(file, data[, options])` - sync version of writes data to a file, replacing the file if it already exists. data can be a string or a buffer.  

### os (operative system)

`const os = require('os')`  

`console.log('Hostname: ' + os.hostname())`  
`console.log('OS type: ' + os.type())`  
`console.log('OS platform: ' + os.platform())`  
`console.log('OS release: ' + os.release())`  
`console.log('OS uptime: ' + (os.uptime() / 60 / 60 / 24).toFixed(1) + ' days')`  
`console.log('CPU architecture: ' + os.arch())`  
`console.log('Number of CPUs: ' + os.cpus().length)`  
`console.log('Total memory: ' + (os.totalmem() / 1e6).toFixed(1) + ' MB')`  
`console.log('Free memory: ' + (os.freemem() / 1e6).toFixed(1) + ' MB')` 

---

## MIDDLEWARES

### Types

* **Application level**  

```javascript
const app = express();
app.use() 
app.METHOD() [METHOD = GET, PUT, POST, UPDATE ...]
```

* **Router level**  
```javascript
const router = express.Router();
router.use() 
router.METHOD() [METHOD = GET, PUT, POST, UPDATE ...]
```


* **Error Handling** One more argument (err)   
```javascript
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Error : something broke!');
});
```

* **Built-in**  

`express.static` - serves static assets such as HTML files, images, and so on.  
`express.json``- parses incoming requests with JSON payloads. NOTE: Available with Express 4.16.0+  
`express.urlencoded` - parses incoming requests with URL-encoded payloads. NOTE: Available with Express 4.16.0+  

```javascript

```

* **Third Party**  
```javascript
npm install middleware;
const middleware = require('middleware');
app.use(middleware());
```

### Middlewares

[https://expressjs.com/en/resources/middleware.html](https://expressjs.com/en/resources/middleware.html)

---

## REQUEST-RESPONSE

### Request

[Request API](https://expressjs.com/en/4x/api.html#req)

**`req.query`** - This property is an object containing a property for each query string parameter in the route. If there is no query string, it is the empty object, {}.  
```js
// GET /search?q=jolav
req.query.q // => "jolav"

// GET /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
req.query.order   // => "desc"
req.query.shoe.color  // => "blue"
req.query.shoe.type // => "converse"
```
 
**`req.params`** - This property is an object containing properties mapped to the named route “parameters”. For example, if you have the route /user/:name, then the “name” property is available as req.params.name. This object defaults to {}.
```js
// GET /user/jolav on route /user/:name
req.params.name // => "jolav"

// GET /file/js/react.js on royte /file/*
req.params[0]
// => "js/react.js"
```

**`req.body`** - Contains key-value pairs of data submitted in the request body. By default, it is undefined, and is populated when you use body-parsing middleware such as body-parser and multer.  

**`req.route`** - Contains the currently-matched route, a string  
```js
app.get('/user/:id?', function userIdHandler(req, res) {
  console.log(req.route);
  res.send('GET');
});

{ path: '/user/:id?',
  stack:
   [ { handle: [Function: userIdHandler],
       name: 'userIdHandler',
       params: undefined,
       path: undefined,
       keys: [],
       regexp: /^\/?$/i,
       method: 'get' } ],
  methods: { get: true }  }
```

**`req.cookies`** - When using cookie-parser middleware, this property is an object that contains cookies sent by the request. If the request contains no cookies, it defaults to {}.  

**`req.headers`** -  

**`req.xhr`** - A Boolean property that is true if the request’s X-Requested-With header field is “XMLHttpRequest”, indicating that the request was issued by a client library such as jQuery.   

**`req.hostname`** - Contains the hostname derived from the Host HTTP header.  
```js
// Host: "example.com:3000"
req.hostname
// => "example.com"
``` 

**`req.originalUrl - req.path - req.baseUrl`** -
```js
// GET /search?q=something
req.originalUrl  // => "/search?q=something"

// example.com/users?sort=desc
req.path // => "/users"

// GET 'http://www.example.com/admin/new'
app.use('/admin', function(req, res, next) {  
  console.log(req.originalUrl); // '/admin/new'
  console.log(req.baseUrl); // '/admin'
  console.log(req.path); // '/new'
  next();
});
``` 

**`req.protocol`** - Contains the request protocol string: either http or (for TLS requests) https.   



### Response

[Response API](https://expressjs.com/en/4x/api.html#res)

**`res.headersSent`** - Boolean property that indicates if the app sent HTTP headers for the response.  
```js
app.get('/', function (req, res) {
  console.log(res.headersSent); // false
  res.send('OK');
  console.log(res.headersSent); // true
});
```

**`res.cookie(name, value [, options])`** - Sets cookie name to value. The value parameter may be a string or object converted to JSON. The options parameter is an object that can have the following properties (domain, encode, expires, httpOnly, maxAge, path, secure, signed ,sameSite)  

**`res.download(path [, filename] [, options] [, fn])`** - Transfers the file at path as an "attachment". Typically, browsers will prompt the user for download.  
```js
res.download('/report-12345.pdf', 'report.pdf', function(err){
  if (err) {
    // Handle error, response may be partially-sent, check res.headersSent
  } else {
    // decrement a download credit, etc.
  }
});
``` 

**`res.end([data] [, encoding])`** - Ends the response process. Use to quickly end the response without any data. If you need to respond with data, instead use methods such as res.send() and res.json().  

**`res.render(view, [locals], callback)`** - Renders a view and sends the rendered HTML string to the client.  

**`res.set(filed, [value])`** - Sets the response’s HTTP header field to value. To set multiple fields at once, pass an object as the parameter.  
```js
res.set('Content-Type', 'text/plain');
res.set({
  'Content-Type': 'text/plain',
  'Content-Length': '123',
  'ETag': '12345'
});
res.setHeader('Content-Type', 'application/json');
// res.header() is an alias of res.set() method from Express framework.
// The only difference is res.setHeader() allows you only to set a singular
// header and res.header() will allow you to set multiple headers
// res.setHeader() is a native method of Node.js
```   

**`res.status(code)`** - Sets the HTTP status for the response. It is chainable  
```js
res.status(200).send(JSON.stringify(data, null, 3)); 
// same as 
res.status(200).json(data);
```

**`res.send([status], body)`** - Sends the HTTP response. The body parameter can be a Buffer object, a String, an object, or an Array  
```js
res.send(new Buffer('whoop'));
res.send({ some: 'json' });
res.send('<p>some html</p>');
res.status(404).send('Sorry, we cannot find that!');
res.status(500).send({ error: 'something blew up' });
```
 
**`res.sendFile(path [, options] [, fn])`** - Transfers the file at the given path. Sets the Content-Type response HTTP header field based on the filename’s extension. Unless the root option is set in the options object, path must be an absolute path to the file.  
```js
app.get('/file/:name', function (req, res, next) {
  const options = {
    root: __dirname + '/public/',
    dotfiles: 'deny',
    headers: {
        'x-timestamp': Date.now(),
        'x-sent': true
    }
  };
  const fileName = req.params.name;
  res.sendFile(fileName, options, function (err) {
    if (err) {
      next(err);
    } else {
      console.log('Sent:', fileName);
    }
  });
})
```

**`res.json([status], json)`** - Sends a JSON response. This method sends a response (with the correct content-type) that is the parameter converted to a JSON string using JSON.stringify().    

**`res.jsonp([status], jsonp)`** - Sends a JSON response with JSONP support. This method is identical to res.json(), except that it opts-in to JSONP callback support.  

**`res.redirect([status], path)`** - Redirects to the URL derived from the specified path, with specified status, a positive integer that corresponds to an HTTP status code . If not specified, status defaults to “302 “Found”.  

**`res.location(path)`** - Sets the response Location HTTP header to the specified path parameter.  
```js
res.redirect('/foo/bar');
res.redirect('http://example.com');
res.redirect(301, 'http://example.com');
res.redirect('../login');
```

---

## ASYNC-AWAIT


test1
```js
async function doSomething() {
  //...
  const rate = await getApiLimit(); 
  // avoid unhandled promise rejection with
  const rate = await getApiLimit().catch(() => {}); 
  // or 
  try {
    const rate = await getApiLimit();
  } catch(){
    //...
  }
}

function getApiLimit() {
  return new Promise((resolve, reject) => {
    const options = {
      hostname: 'api.github.com',
      port: 443,
      path: "/rate_limit",
      method: 'GET',
      headers: {
        'Authorization': 'Bearer ' + token[lib.getRandomNumber(0, 9)],
        "User-Agent": "github stars repos",
        'Accept': '/application/vnd.github.v3.star+json',
      }
    };
    lib.makeRequest(options, null, function (err, data, headers) {
      if (err) {
        console.log('Error => ', err);
        reject(-1);
      }
      resolve(data);
    });
  });
}
```

### Patterns

Anonymous Async Function

```js
let main = (async function() {
  const data = await myAsyncFunc();
})();
```

Async Function Declaration

```js
async function main() {
  const data = await myAsyncFunc();
};
```

Async Function Assignment

```js
let main = async function() {
  const data = await myAsyncFunc();
};

let main = async () => {
  const data = await myAsyncFunc();
};
```

Object & Class Methods

```js
// Object property
let obj = {
  async method() {
    const data = await myAsyncFunc();
  }
};

// Class methods
class MyClass {
  async myMethod() {
    const data = await myAsyncFunc();
  }
}
```

### Error Handling

```js
try {
  const data = await myAsyncFunc();
}
catch(e) {
 // Error!
}
```

### Parallelism

```js
// Will take 1000ms total!
async function series() {
  await wait(500);
  await wait(500);
  return "done!";
}

// Would take only 500ms total!
async function parallel() {
  const wait1 = wait(500);
  const wait2 = wait(500);
  await wait1;
  await wait2;
  return "done!";
}

async function parallel() {
  const [wait1, wait2] = await Promise.all([
    wait(500),
    wait(500),
  ]);
  return "done!";
}
```

---

