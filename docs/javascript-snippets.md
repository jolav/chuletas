# JAVASCRIPT SNIPPETS

## NUMBERS

### Random Numbers

```javascript
function randomInt (min, max) {
  return Math.floor(Math.random() * (max - min + 1) + min);
}
```

### areAllCharsNumbers

```javascript
function areAllCharsNumbers (str) {
  return str.match(/^\d+$/);
}
```

### remove trailing zeroes

```js
const num = 6.76789;
num.toFixed(2);  // return 6.77

const num = 6;
num.toFixed(3); //  return 6.000

Number(num.toFixed(2));     // return 6.77 , 6
parseFloat(num.toFixed(3)); // return 6.77 , 6
```

---

## STRINGS

### Create UUIDv4

```js
// vale para el navegador y para nodejs
function generateUUID() {
  const template = [1e7] + -1e3 + -4e3 + -8e3 + -1e11;
  const uuid = template.replace(/[018]/g, function (c) {
    const random = crypto.getRandomValues(new Uint8Array(1))[0];
    const value = (c ^ random & 15 >> c / 4).toString(16);
    return value;
  });
  return uuid;
}
```

### Clean whitespaces

```javascript
function removeAllWhitespaces (str) {
  return str.replace(/\s+/g, '');
}
```

```javascript
function replaceAllWhitespacesByChar (str, otherChar) {
  return str.replace(/\s+/g, otherChar);
}
```

```js
function replaceMultipleWhitespacesWithASingleSpace(str) {
  return str.replace(/\s\s+/g, ' ');
}
```

### Remove empty strings

Eliminar las cadenas vacias de un array

```js
let arr = ["I", "am", "", "still", "here", "", "man"]
// arr = ["I", "am", "", "still", "here", "", "man"]

// ES5
arr = arr.filter(Boolean)
// ES6
arr = arr.filter(v=>v!='');
```

---

## ARRAYS

### InitializeMultiArray

```javascript
function initializeMultiArray (cols, rows, value) {
  let array = [];
  for (let i = 0; i < cols; i++) {
    array[i] = [];
    for (let j = 0; j < rows; j++) {
      array[i][j] = value;
    }
  }
  return array;
}
```

### Order Array by Property  

```javascript
function orderArrayByPropertyName (property) {
  let sortOrder = 1;
  if (property[0] === '-') {
    sortOrder = -1;
    property = property.substr(1);
  }
  return function (a, b) {
    let result = (a[property] < b[property]) ? -1 :
              (a[property] > b[property]) ? 1 : 0;
    return result * sortOrder;
  };
}
```

---

## OBJECTS

### isEmpty(Object)

```js
function objectIsEmpty(obj) {
  return Object.keys(obj).length === 0 && obj.constructor === Object;
}
```

---

## FETCH

```js
async function fetchData(c, data) {
  const options = {
    method: 'GET',
    signal: AbortSignal.timeout(5000),
    headers: {
      'Authorization': `Bearer ${"token"}`,
      'Accept-Charset': 'utf-8',
    },
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

async function fetchData(c, data) {
  const options = {
    method: 'POST',
    signal: AbortSignal.timeout(5000),
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Authorization': `Bearer ${"token"}`,
      'Accept-Charset': 'utf-8'
    },
    body: new URLSearchParams({
      'key': c.API_KEY, //o el token en el header
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

//  Si la API espera JSON, 
// cambiar el "Content-Type" : "application/json"
//  y convertir body a JSON con JSON.stringify()
```

---

## AJAX REQUESTS

### GET 

```javascript
function getAjaxData (urlData, callback) {
  const xhr = new XMLHttpRequest();
  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) { // 4 = "DONE"
      if (xhr.status === 200) { // 200 ="OK"
        callback(JSON.parse(xhr.responseText));
      } else {
        console.log('Error: ' + xhr.status);
      }
    }
  };
  xhr.open('GET', urlData); // add false to synchronous request
  xhr.send();
}
```

### GET, POST, PUT ...   

```javascript
function makeAjaxRequest (url, action, params, sendCookie , callback) {
  const xhr = new XMLHttpRequest();
  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) { // 4 = "DONE"
      if (xhr.status === 200) { // 200 ="OK"
        if (action === 'GET' || action === 'POST') {
          try {
            callback(JSON.parse(xhr.responseText));
          } catch (e) {
            console.log('Error parsing Json => ', e);
            callback({});
          }
        } else {
          callback(xhr.status);
        }
      } else {
        console.log('Error: ' + xhr.status);
      }
    }
  };
  xhr.open(action, url);
  if (sendCookie) {
    xhr.withCredentials = true; // allow send cookies
  }
  if (action === 'GET') {
    xhr.send();
  } else if (action !== 'GET') {
    xhr.setRequestHeader('Content-Type',
      'application/x-www-form-urlencoded; charset=UTF-8');
    if (params) {
      xhr.send(params);
    } else {
      xhr.send();
    }
  // console.log('Request Sent')
  }
}
```

### ASYNC/AWAIT REQUEST

```js
async function init() {
  try {
    const data = await makeRequest(url, 'POST', param);
  } catch (err) {
    console.error("ERROR FETCHING DATA => ", err);
  }
}

function makeRequest(url, method, param) {
  return new Promise(function (resolve, reject) {
    let xhr = new XMLHttpRequest();
    xhr.open(method, url);
    xhr.onload = function () {
      console.log(xhr.status);
      if (xhr.status >= 200 && xhr.status < 300) {
        resolve(xhr.response);
      } else {
        reject({
          status: xhr.status,
          statusText: xhr.statusText
        });
      }
    };
    xhr.onerror = function () {
      reject({
        status: xhr.status,
        statusText: xhr.statusText
      });
    };
    if (method === 'GET') {
      xhr.send();
    } else if (method !== 'GET') {
      xhr.setRequestHeader('Content-Type', 
      'application/x-www-form-urlencoded; charset=UTF-8');
      if (param) {
        xhr.send(param);
      } else {
        xhr.send();
      }
    }
  });
}
```

### Wait Multiple Requests

```js
let calls = numberOfCalls;
for (let i = 0; i < calls; i++) {
  asyncOperation(params, function (result) {
    calls--;
    if (calls <= 0) {
      // all calls done  
    }
  });
}
```

---

## NETWORK

### Validate IP 

```javascript
function isValidIP(ip) {
  const ip4 = /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;
  const ip6 = /(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))/;
  if (ip4.test(ip)) {
    return true;
  }
  if (ip6.test(ip)) {
    return true;
  }
  return false;
}
```

### Validate Hostname

```javascript
function isValidHostname(hostname) {
  if (!hostname || hostname.length > 255) {
    return false;
  }
  const condition = /^(?!-)[A-Za-z0-9-]{1,63}(?<!-)$/;
  const parts = hostname.split('.');
  for (let part of parts) {
    if (!condition.test(part)) {
      return false;
    }
  }
  return true;
}
```

### Validate URL

```javascript
function isValidURL(url) {
  const valid = ["http", "https"];
  const invalid = ["localhost", "127.0.0.1"];
  try {
    const myUrl = new URL(url);
    const protocol = myUrl.protocol.replace(":", "");
    const hostname = myUrl.hostname;
    return valid.includes(protocol) && !invalid.includes(hostname);
  } catch (err) {
    return false;
  }
}
```

### Remove protocol-www

```js
function cleanUrl(url) {
  if (url.startsWith('https://')) {
    url = url.slice(8);
  } else if (url.startsWith('http://')) {
    url = url.slice(7);
  }

  if (url.startsWith('www.')) {
    url = url.slice(4);
  }
  return url;
}
```

--- 

## VARIOS

### Set daily task

```javascript
function onceADayTask () {
  let now = new Date();
  let target = new Date(
    now.getFullYear(),
    now.getMonth(),
    now.getDate() + 1, // the next day, ...
    5, 0, 0 // ...at 05:00:00 hours server local time
  );
  let msToTask = target.getTime() - now.getTime();

  setTimeout(function () {
    // do whatever you want
    onceADayTask();
  }, msToTask);
}
```

### Check Date valid

fails 30-31 feb, 31 apr and so on ...

```js
if (isNaN(new Date(date).getTime())) {
  return 'incorrect date'; // date not valid date
}
```

### Random Colors

```javascript
function getRandomColor () {
  let letters = '0123456789ABCDEF'.split('');
  let color = '#';
  for (let i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}
```

### Check Image exists

```js
const image = new Image();
image.onload = () => {
  // exists
};
image.onerror = () => {
  // not exists
};
image.src = linkToImage;
```

### Check isJSON

```js
function isJSON(data) {
  try {
    JSON.parse(data);
  } catch (e) {
    return false;
  }
  return true;
}
```

---

## DATE

### AddDays to Date

```js
function addDays (date, days) {
  let result = new Date(date);
  result.setDate(result.getDate() + days);
  return result;
}
```
