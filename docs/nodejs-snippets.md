# NODEJS SNIPPETS

---

### ES MODULES  
Hasta que sean nativos en nodejs 21.X la opcion es poner en package.json
```json
{
  "type": "module"
}
```
o usar la extension mjs en lugar de js.  

---

## FETCH

```js
async function fetchData(c, data) {
  const options = {
    timeout: c.timeout,
    host: c.host,
    port: c.port,
    path: c.api,
    method: 'POST',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Accept-Charset': 'utf-8'
    },
    body: new URLSearchParams({
      'key': c.API_KEY,
      'data': JSON.stringify(data)
    }),
  };
  const url = c.API_URL + c.API_ENDPOINT;
  try {
    const response = await fetch(url, options);
    if (response.ok) {
      const data = await response.json();
      return data;
    } else {
      throw new Error(response.status + " " + response.statusText);
    }
  } catch (err) {
    console.log('ERROR fetchData => ', err);
  }
}
```

---

## HTTP Request

### GET

```javascript
function makeHttpsRequest (path, callback) {
  https.get(path, (res) => {
    res.setEncoding('utf8');
    var body = '';
    res.on('data', (d) => {
      body += d;
    });
    res.on('end', () => {
      try {
        var parsed = JSON.parse(body);
      } catch (err) {
        console.error('Unable to parse response as JSON', err);
        return callback(err, null, null);
      }
      callback(null, res, parsed);
    });
  }).on('error', (err) => {
    console.error('Error with the request:', err.message);
    callback(err, null, null);
  });
}
```

### POST, PUT ...

```js
const querystring = require('querystring');

const parameters = querystring.stringify({
  'test': c.app.valid,
  'server': data.server,
  'data': data.data
});

const options = {
  timeout: 3000,
  hostname || host: 'api.codetabs.com',
  port: 443,
  path: '/api/resource',
  method: 'GET',
  headers: {
    'Authorization': 'Bearer ' + process.env.TOKEN_0,
    "Content-Type": 'x-www-form-urlencoded',
    "User-Agent": "user agent",
    'Accept': 'application/json',
    'Content-Length': Buffer.byteLength(parameters),
    'Accept-Charset': 'utf-8'
  }
};

makeRequest(options, parameters, function (err, res, data) {
  if (err !== null) {
    console.error('data sent FAIL => ', err);
    return;
  }
  console.log('data sent OK', data.server);
});

function makeRequest(options, parameters, callback) {
  const req = https.request(options, function (res) {
    let body = '';
    res.setEncoding('utf8');
    res.on('data', function (d) {
      body += d;
      // save all the data from response
      //console.log('BODY =>', body);
    });
    res.on('end', function () {
      //console.log('RES END');
      try {
        //console.log(body);
        var parsed = JSON.parse(body);
      } catch (err) {
        console.error('Unable to parse response as JSON', err);
        return callback(err, null, null);
      }
      callback(null, res, parsed);
    });
  });
  if (options.method !== 'GET') {
    req.write(parameters);
  }
  req.on('timeout', function () {
    //console.error('TIMEOUT');
    req.abort(); // lo envia a abort
  });
  req.on("abort", function () {
    callback("TIMEOUT", null, null);
  });
  req.on('error', function (err) {
    console.error('Error request  => ', err);
    // para evitar que salte 
    // Error: socket hang up
    // code: 'ECONNRESET'
    //console.log(err);
  });
  req.end();
}


// ASYNC POST REQUEST
try {
  const myRequest = await makeRequestAsync(options, parameters);
  console.log(myRequest);
} catch (err) {
  console.log(err);
}

function makeRequestAsync(options, parameters) {
  return new Promise((resolve, reject) => {
    const req = https.request(options, function (res) {
      let body = '';
      res.setEncoding('utf8');
      res.on('data', function (d) {
        body += d; // save all the data from response
      });
      res.on('end', function () {
        try {
          var parsedResponse = JSON.parse(body);
        } catch (err) {
          console.error('Unable to parse response as JSON', err);
          reject(err);
        }
        resolve(parsedResponse);
      });
    });
    if (options.method !== 'GET') {
      req.write(parameters);
    }
    req.on('timeout', function () {
      req.abort(); // send req to abort
    });
    req.on("abort", function () {
      reject("TIMEOUT");
    });
    req.on('error', function (err) {
      console.error('Error request  => ', err);
      // Avoid -> Error: socket hang up code: 'ECONNRESET'
    });
    req.end();
  });
}
```

---

## FILES

### read JSON

```javascript
// sincrona
import { readFileSync } from 'fs';

function loadJsonFile() {
  try {
    const data = readFileSync('./data.json', 'utf8');
    return JSON.parse(data);
  } catch (err) {
    console.error('Error reading JSON file =>', err);
    return null;
  }
}

// asincrona
import { promises as fs } from 'fs';

async function loadJsonFile() {
  try {
    const data = await fs.readFile('./data.json', 'utf8');
    return JSON.parse(data);
  } catch (err) {
    console.error('Error reading JSON file =>', err);
  }
}
```

### write JSON

```javascript
// sincrona
import { writeFileSync } from 'fs';

function saveJsonFile(jsonData) {
  try {
    const data = JSON.stringify(jsonData, null, 2);
    writeFileSync('./data.json', data, 'utf8');
  } catch (err) {
    console.error('Error saving JSON file =>', err);
  }
}

// asincrona
import { promises as fs } from 'fs';

async function saveJsonFile(jsonData) {
  try {
    const data = JSON.stringify(jsonData, null, 2);
    await fs.writeFile("./data.json", data);
  } catch (err) {
    console.error("Error Saving JSON file => ", err);
  }
}
```

### Get file from url  

Descargar archivo a nuestro servidor con expressjs desde una url

```js
const fs = require('fs');
const https = require('https');

function downloadFile (url, where, cb) {
  const file = fs.createWriteStream(where);
  https.get(url, function (response) {
    response.pipe(file);
    response.on('end', function (end) {
      cb(true);
    });
    response.on('error', function (err) {
      console.log('ERROR=', err);
      cb(false);
    });
  });
}
```

### Readline file

```js
const fs = require("fs");
const readline = require("readline");

function countLocFromTxt(filePath, cb) {
  const lineReader = readline.createInterface({
    input: fs.createReadStream(filePath)
  });
  let lineNumber = 1;
  lineReader.on('line', function (line) {
    console.log(lineNumber);
    lineNumber++;
  });
  lineReader.on('close', function () {
    cb(data);
  });
}
```

### Upload File

`npm install --save formidable`

```html
<form class="upload" method="post" enctype="multipart/form-data">
  <input type="file" class="input" id="inputFile" name="inputFile">
  <button type="submit" id="upload" class="action">UPLOAD</button>
</form>
```

```js
const fs = require("fs");
const formidable = require('formidable');

const form = new formidable.IncomingForm();
form.parse(req, function (err, fields, files) {
  const filename = files.inputFile.name;
  const oldpath = files.inputFile.path;
  const newpath = __dirname + "/desired/path/uploads" + filename;
  fs.rename(oldpath, newpath, function (err) {
    if (err) throw err;
  });
});
```

---

## NETWORK

### get IP

```javascript
function getIP(req) {
  return (
    req.headers['x-forwarded-for'] ||
    req.connection.remoteAddress ||
    req.socket.remoteAddress ||
    req.connection.socket.remoteAddress).split(',')[0];
}
```

### check url exists

```js
const https = require('https');
const url = require('url');

function checkUrlExists (urlchecked, cb) {
  const options = {
    method: 'HEAD',
    host: url.parse(urlchecked).host,
    path: url.parse(urlchecked).path
  };
  const req = https.get(options, function (response) {
    // console.log('STATUS=>', response.statusCode)
    cb(response.statusCode === 200);
  });
  req.end();
  req.on('error', function (err) {
    console.log('Error checking url => ', err);
    cb(false);
  });
}
```

---


## ASYNC/AWAIT

```js
// Ejemplo 1
function sleep(sleepTime) {
  return new Promise(function (resolve, reject) {
    setTimeout(resolve, sleepTime);
  });
}

async function go() {
  console.log('Starting');
  await sleep(1000);
  console.log('Ending');
}

// Ejemplo 2
async function doThings() {
  try {
    const exist = await existsRepo(url);
    console.log(exists);
  } catch(err) {
    console.error(err);
  }
}

function existsRepo (urlchecked) {
  return new Promise(function (resolve, reject) {
    lib.checkUrlExists(urlchecked, function (err, res) {
      if (err) {
        resolve(err);
      }
      resolve(res);
    });
  });
}
```

### paralelismo  

```js
// Asi tarda 1000ms
async function series() {
  console.log("series");
  await wait(500);
  await wait(500);
  console.log("series done");
}

// Asi tarda 500ms
async function parallel() {
  console.log('parallel');
  const wait1 = wait(500);
  const wait2 = wait(500);
  await wait1;
  await wait2;
  console.log('parallel done');
}

// Asi tarda 500ms
async function parallel2() {
  console.log("parallel2");
  const [wait1, wait2] = await Promise.all([
    wait(500),
    wait(500),
  ]);
  console.log('parallel2 done');
}

series();
parallel();
parallel2();
```

---

## MISC

### Linux Commands

#### Sincronos  

```js
import { execSync } from "child_process";

const createNewDir = `cd ${origin} && mkdir ${newDir}`;

try {
  const modes = ["dev", "production"];
  const mode = modes[action];
  if (mode === "dev") {
    console.log(createNewDir);
  }
  if (mode === "production") {
    execSync(createNewDir);
  }
} catch (err) {
  console.error('Error =>', err);
}
```

#### Asincronos  

```js

import { exec } from 'child_process';

// callbacks
function runCommandCB(command, cb) {
  exec(command, function (err, stdout, stderr) {
    if (err) {
      console.error(`Error executing "${command}" =>`, err);
      cb(err, stderr || stdout);
      return;
    }
    if (stderr) {
      console.warn(`Warn executing "${command}" =>`, stderr);
      cb(null, stderr);
      return;
    }
    cb(null, stdout);
  });
}

runCommandCB(command, function (err, output) {
  if (err) {
    console.log("Err => ", err);
  } else {
    console.log("Output => ", output);
  }
});

// promises
function runCommandPR(command) {
  return new Promise(function (resolve, reject) {
    exec(command, function (err, stdout, stderr) {
      if (err) {
        console.error(`Error executing "${command}" => `, err);
        reject(err);
      } else if (stderr) {
        console.warn(`Warn executing "${command}" => `, stderr);
        resolve(stdout);
      } else {
        resolve(stdout);
      }
    });
  });
}

runCommandPR(command)
  .then(function (output) {
    console.log("Output => ", output);
  })
  .catch(function (err) {
    console.log("Err => ", err);
  });
```

### pm2 cluster 

* **Buscar un proceso**

```js
if (process.env && process.env.pm_id) {
  if (process.env.pm_id === '0') {
    onceADayTask();
  }
}

if (process.env && process.env.pm_id) {
  //running in pm2 
  if (process.env.pm_id % os.cpus().length !== 0) {
    return;
  } else {
    doTask();
  }
}
```

---


