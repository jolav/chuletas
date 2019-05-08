# NODEJS SNIPPETS

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
const options = {
  hostname: 'api.codetabs.com',
  port: 443,
  path: '/api/resource',
  method: 'GET',
  headers: {
    'Authorization': 'Bearer ' + process.env.TOKEN_0,
    "Content-Type": 'x-www-form-urlencoded',
    "User-Agent": "user agent",
    'Accept': 'application/json',
  }
};

function makeRequest (options, parameters, callback) {
  const req = https.request(options , function (res) {
    let body = '';
    res.setEncoding('utf8');
    res.on('data', function (d) {
      body += d;
    // save all the data from response
    });
    res.on('end', function () {
      try {
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
  req.end();
}
```

---

## FILES

### read JSON

```javascript
function loadJSONfile (filePath, callback) {
  fs.readFile(filePath, 'utf8', (err, data) => {
    if (err) {
      console.log('ERROR READING JSON FILE => ', err);
      throw err;
    } else {
      callback(JSON.parse(data));
    }
  });
}
```

### write JSON

```javascript
function writeJSONtoFile (filePath, dataSet, callback) {
  const json = JSON.stringify(dataSet);
  fs.writeFile(filePath, json, 'utf8', (err) => {
    if (err) {
      console.log('ERROR WRITING JSON IN FILE => ', err);
      throw err;
    }
    callback();
  });
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
function getIP (req) {
  return (req.headers['x-forwarded-for'] ||
  req.connection.remoteAddress || req.socket.remoteAddress ||
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

async function doThings() {
  const exist = await existsRepo(url);
  console.log(exists);
}

function existsRepo (urlchecked) {
  return new Promise((resolve, reject) => {
    lib.checkUrlExists(urlchecked, function (err, res) {
      if (err) {
        resolve(err);
      }
      resolve(res);
    });
  });
}
```

---

## MISC

### Linux Commands

```js
const exec = require('child_process').exec;

function linuxCommand (command, cb) {
  exec(command, function (err, stdout, stderr) {
    if (err) {
      // console.log('Err => ', err)
      cb(err, stderr);
    }
    if (stderr) {
      cb(err, stderr);
      return;
    }
    cb(err , stdout);
  });
}
```

### MongoDB Backups

```js
// crontab -e
// 0 */12 * * * node /var/www/path/to/backup/backup.js // every 12 hour

// la primera vez ejecutarlo manualmente para evitar que pregunte por el 
// fingerprint y se quede pillado

require('dotenv').config();
const exec = require('child_process').exec;
mongoexport();

function mongoexport () {
  const user = process.env.DBUSER;
  const password = process.env.PASS;
  const dbname = process.env.DB_NAME;
  const collection = [process.env.COLLECTION1, process.env.COLLECTION2];
  for (let i = 0; i < collection.length; i++) {
    const command = `mongoexport --db ${dbname} 
    --collection ${collection[i]}
    --out data/${collection[i]}.json -u '${user}' -p '${password}' 
    --authenticationDatabase 'admin'`;
    exec(command, function (err, stdout, stderr) {
      if (err) {
        console.log('Err => ', err);
        return;
      }
      if (stderr) {
        // console.log('StdErr => ', i /*stderr*/)
        // return
      }
      // console.log('Stdout =>', i /* stdout*/)
      if (i === collection.length - 1) {
        scp();
      }
    });
  }
}

function scp () {
  const path = process.env.MYPATH; // path/to/file/with/password
  // remote 'username@domain.com:/path/to/backup'
  const remote = process.env.REMOTE; 
  const command = `sshpass -f ${path} scp -r data ${remote}`;
  exec(command, function (err, stdout, stderr) {
    if (err) {
      console.log('Err => ', err);
      return;
    }
    if (stderr) {
      // console.log('StdErr => ', stderr)
      // return
    }
  // console.log('DONE =>', stdout)
  });
}
```

### pm2 cluster 

* **Buscar un proceso**

```js
if (process.env && process.env.pm_id) {
  if (process.env.pm_id === '0') {
    onceADayTask()
  }
}
```

---


