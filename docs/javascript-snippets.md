# JAVASCRIPT SNIPPETS

## NUMBERS

### Random Numbers

```javascript
function getRandomNumber (min, max) {
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

### Send Cookies to APIs

```js
// Server
app.use(function (req, res, next) {
  // next line for avoid CORS
  res.header('Access-Control-Allow-Origin', req.headers.origin);
  // cookies
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With,
   Content-Type, Accept');
  res.header('Access-Control-Allow-Credentials', true);
  next();
});

// client
xhr.withCredentials = true; // allow send cookies
```

---

## NETWORK

### Validate IP 

```javascript
function isValidIP (ip) {
  if (/^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(ip)) {
    return true;
  } else if (/(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|fe80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::(ffff(:0{1,4}){0,1}:){0,1}((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9])\.){3,3}(25[0-5]|(2[0-4]|1{0,1}[0-9]){0,1}[0-9]))/.test(ip)) {
    return true;
  } else {
    return false;
  }
}
```

### Validate Hostname

```javascript
function isValidHostname (hostname) {
  let condition = /^(([a-zA-Z0-9]|[a-zA-Z0-9][a-zA-Z0-9\\-]*[a-zA-Z0-9])\.)*([A-Za-z0-9]|[A-Za-z0-9][A-Za-z0-9\\-]*[A-Za-z0-9])$/;
  if (condition.test(hostname)) {
    return true;
  }
  return false;
}
```

### Validate URL

```javascript

function isValidURL (url) {
  return /^(?:(?:(?:https?|ftp):)?\/\/)(?:\S+(?::\S*)?@)?(?:(?!(?:10|127)(?:\.\d{1,3}){3})(?!(?:169\.254|192\.168)(?:\.\d{1,3}){2})(?!172\.(?:1[6-9]|2\d|3[0-1])(?:\.\d{1,3}){2})(?:[1-9]\d?|1\d\d|2[01]\d|22[0-3])(?:\.(?:1?\d{1,2}|2[0-4]\d|25[0-5])){2}(?:\.(?:[1-9]\d?|1\d\d|2[0-4]\d|25[0-4]))|(?:(?:[a-z\u00a1-\uffff0-9]-*)*[a-z\u00a1-\uffff0-9]+)(?:\.(?:[a-z\u00a1-\uffff0-9]-*)*[a-z\u00a1-\uffff0-9]+)*(?:\.(?:[a-z\u00a1-\uffff]{2,})))(?::\d{2,5})?(?:[/?#]\S*)?$/i.test(url);
}
```

### Get URL Parameter

```javascript
function getURLParameter (name) {
  return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)').exec(location.search) || [null, ''])[1].replace(/\+/g, '%20')) || null;
}
```

### Remove protocol-www

```js
function cleanUrl (url) {
  if (url.slice(0, 12) === 'https://www.') {
    url = url.slice(12);
  } else if (url.slice(0, 11) === 'http://www.') {
    url = url.slice(11);
  } else if (url.slice(0, 8) === 'https://') {
    url = url.slice(8);
  } else if (url.slice(0, 7) === 'http://') {
    url = url.slice(7);
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
