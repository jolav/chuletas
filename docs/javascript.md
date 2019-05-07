# JAVASCRIPT

---

## TYPES

* **Conversions**

```js
Number(str) // string to number
parseInt(str) // string to integer
parseFloat(str) // string to float 
String(num) // number to string
num.toString() // number to string
arr.toString() // array to string with elements separated with commas
```

### Number

`isNan(number)` - determines whether a value is an illegal number   
`number.toFixed(n)` - convert number into a string with -n- decimals  
`number.toPrecision(n)` - format a number into a specified length. Decimal points are created if needed.  

### String

**`String.fromCharCode()`** - converts Unicode values to characters  
```javascript
var code = 65;
var letter = String.fromCharCode(code);   // letter = "A"
```

**`string.charAt(pos)`** - Returns the character at the specified position
```javascript
var name = 'Curly';
var initial = name.charAt(2);   // Pos 2 is 'r'
```

**`string.charCodeAt(pos)`** - Returns the Unicode of the character at the specified position  
```javascript
var name = 'Curly';
var initial = name.charCodeAt(0);   // initial is 67
```

**`string.concat(str1, str2, ...)`** - Joins two or more strings, and returns a new joined strings   
```javascript
var s = 'C'.concat('a', 't');       // s is 'Cat'
```

**`string.indexOf(searchThis, pos)`** - Returns the position of the first found occurrence of a specified value in a string   
```javascript
var text = 'Mississippi';
var p = text.indexOf('ss');   // p is 2
p = text.indexOf('ss', 3);    // p is 5
p = text.indexOf('ss', 6);    // p is -1
```

**`string.lastIndexOf(searchThis, lastPosToSearch)`** - Returns the position of the last found occurrence of a specified value in a string   
```javascript
"canal".lastIndexOf("a")      // returns 3
"canal".lastIndexOf("a",2)    // returns 1
"canal".lastIndexOf("a",0)    // returns -1
```

**`string.replace(searchThis, newValue)`** - Searches a string and returns a new string where the specified values are replaced   
```javascript
var str = "Have a tea";
var res = str.replace("tea", "coffee"); // "Have a coffee"
```

**`string.search(regexp)`** - Searches a string and returns the position of the match
```javascript
var text = 'Tomorrow " i will go';
var pos = text.search(/["']/);    // pos is 10
```

**`string.slice(start, end)`** - Extracts a part of a string and returns a new string  
```javascript
var str = 'Hello World';
var res = str.slice(1, 4);   // res is 'ell'
var res = str.slice(2);      // res is 'llo World'
```

**`string.split(separator, hoyManyTimes)`** - Splits a string into an array of substrings   
```javascript
var str = "What are you doing";
var res = str.split(" ", 4)  // res ['What','are','you','doing']
```

**`string.substring(start, end)`** - Extracts the characters from a string, between two specified indices     
**`string.toLowerCase()`** - Converts a string to lowercase letters    
**`string.toUpperCase()`** - Converts a string to uppercase letters     

**`string.fromCharCode(car1, car2, ...)`** - Converts Unicode values to characters 
```javascript
var a = String.fromCharCode(67, 97, 116);
// a is 'Cat'
```

**`string.match(regexp)`** - Searches a string for a match against a regular expression, and returns the matches  
```javascript
var str = "The rain in ITALY stays mainly in the plain";
var res = str.match(/ain/g); 
console.log(res) // [ 'ain', 'ain', 'ain' ]
```

**`string.trim(str)`** - Removes whitespace from both sides of a string   

**`string.startsWith("str")`** - returns boolean   
```javascript
const domain = "codetabs.com";
console.log(domain.startsWith("www.")); // false
```

### Boolean

* **TERNARY OPERATOR**

```javascript
test ? expression1 : expression2  
// test true => expression1 // test false => expression2

var now = new Date();  
var greeting = "Good" + ((now.getHours() > 17) ? " evening." : " day.");
```

### Null - undefined

Null is for Objects  
Undefined is for properties, methods or vars not yet initializated.

```javascript
// check if undefined or null
if (x === undefined || x === null) {
  ...
}
// undefined and null are false
if (!x) {
  ...
}
```

### Arrays

* **METHODS**

`D Destructive` - they change the arrays that they are invoked on.  
`ND NonDestructive` - they don’t modify their receivers; such methods often return new arrays  


- **ADD ELEMENTS**

D **`array.push(item1, item2, ...)`** - Adds new elements to the end of an array, and returns the new length
```javascript
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.push(b, true);
// a is['a', 'b', 'c', ['x', 'y', 'z'], true]
// c is 5
```

D **`array.unshift(item1, item2, ...)`** - Adds new elements to the beginning of an array, and returns the new length  
```javascript
var a = ['a', 'b', 'c'];
var r = a.unshift('?', '@');
// a is ['?', '@', 'a', 'b', 'c']
//r is 5
```

- **DELETE ELEMENTS**

D **`array.pop()`** - Removes the last element of an array, and returns that element  
```javascript
var a = ['a', 'b', 'c'];
var c = a.pop(); // a is ['a', 'b'] , c is 'c'
```

D **`array.shift()`** - Removes the first element of an array, and returns that element  
```javascript
var a = ['a', 'b', 'c'];
var c = a.shift();
// a is ['b', 'c'] , c is 'a'
```

- **ITERATION**

ND **`forEach(valor, indice)`** - Calls a function for each array element  
```javascript
var people = [
    {name: 'Casey', rate: 60},
    {name: 'Camille', rate: 80},
    {name: 'Gordon', rate: 75},
    {name: 'Nigel', rate: 120}
];
var results = [];                                
people.forEach(function(person) {                
  if (person.rate >= 65 && person.rate <= 90) {  
    results.push(person);                        
  }
});
```

ND **`some()`** - Checks if any of the elements in an array pass a test. If it finds an array element where the function returns a true value, some() returns true (and does not check the remaining values)  
```javascript
var ages = [3, 10, 18, 20];
function checkAdult(age) {
    return age >= 18;
}
function myFunction() {
    res = ages.some(checkAdult);
}
// res = true
```

ND **`every()`** - Checks if every element in an array pass a test. If it finds an array element where the function returns a false value, every() returns false (and does not check the remaining values)  
```javascript
var ages = [3, 10, 18, 20];
function checkAdult(age) {
    return age >= 18;
}
function myFunction() {
    res = ages.every(checkAdult);
}
// res = false
```

- **MIX**

ND **`array.concat(item1, item2 ...)`** - Joins two or more arrays, and returns a copy of the joined arrays  
```javascript
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.concat(b, true);
// c is ['a', 'b', 'c', 'x', 'y', 'z', true]
```

- **FILTER**

ND **`filter()`** - Creates a new array with every element in an array that pass a test  
```javascript
var people = [
    {name: 'Casey', rate: 60},
    {name: 'Camille', rate: 80},
    {name: 'Gordon', rate: 75},
    {name: 'Nigel', rate: 120}
];
function priceRange(person) {                        
  return (person.rate >= 65) && (person.rate <= 90);
};
var results = [];                              
results = people.filter(priceRange);
```

Callback has three elements  
1. current value  
2. index  
3. Array object we are filtering

```javascript
result = result.filter(function (val, i, res) {
  console.log(val, i, res);
});
```

ND **`reduce()`** - Reduce the values of an array to a single value (going left-to-right). The return value of the function is stored in an accumulator (result/total).  
```javascript
var total = [0, 1, 2, 3].reduce(function(a, b){ return a + b; });
// total == 6

var numbers = [65, 44, 12, 4];
function getSum(total, num) {
    return total + num;
}
console.log(numbers.reduce(getSum));   // resul = 125

var array = [4,5,6,7,8];
var singleVal = 0;
 singleVal = array.reduce(function(previousVal, currentVal) {
  return previousVal + currentVal;
}, 0);
```

- **SEARCH**

ND **`array.indexOf("elemento")`** - Search the array for an element and returns its position or -1 if no exists.  
```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");
// a = 2
```  

ND **`array.lastIndexOf`** - Search the array for an element, starting at the end, and returns its position or -1 if no exists.  
```javascript
var array = [2, 5, 9, 2];
array.lastIndexOf(2);       // 3
array.lastIndexOf(7);       // -1
array.lastIndexOf(2, 3);    // 3
array.lastIndexOf(2, 2);    // 0
```

**`array.includes(value)`** - ES7 - determines whether an array includes a certain element, returning true or false.  
```javascript
function disemvowel (str) {
  var vowels = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'];
  var res = [];
  str.split('').forEach(function (value, index) {
    if (!vowels.includes(value)) res.push(value)
    //if (vowels.indexOf(value) === -1) res.push(value);
  });
  return res.join('');
}
```

- **SORT**

D **`array.sort(funcionDeComparacion)`** - Sorts the elements of an array. By 
default, sorts the values as strings in alphabetical and ascending order.  
```javascript
var n = [4, 8, 15, 16, 23, 42];
n.sort(function (a, b) {
  return a - b;
});
// n is [4, 8, 15, 16, 23, 42];
```

D **`array.reverse()`** - Reverses the order of the elements in an array.  
```javascript
var a = ['a', 'b', 'c'];
var b = a.reverse( );
//  a and b = ['c', 'b', 'a']
```

- **MODIFY**

ND **`map()`** - Creates a new array with the result of calling a function for each array element.  
```javascript
var numbers = [4, 9, 16, 25];
function myFunction() {
  x = numbers.map(Math.sqrt);
// x = [2, 3, 4, 5]
}
```  

ND **`array.join(separator)`** - Joins all elements of an array into a string. The elements will be separated by a specified separator (comma by default).  
```javascript
var a = ['a', 'b', 'c'];
a.push('d');
var c = a.join(''); // c is 'abcd';
```

ND **`array.slice(start, end)`** - Selects a part of an array, and returns the new array starting at the given start argument, and ends at, but does not include, the given end argument.  
```javascript
var a = ['a', 'b', 'c'];
var b = a.slice(0, 1);  // b is ['a']
var c = a.slice(1); // c is ['b', 'c']
var d = a.slice(1, 2); // d is ['b']
```

D **`array.splice(start, howMany, item1, item2, ...)`** - Adds/Removes elements from an array.  
- howMany is the number of items to be removed. If set to 0, no items will be removed  
- item1, item2, ... Optional. The new item(s) to be added to the array   
```javascript
var a = ['a', 'b', 'c'];
var r = a.splice(1, 1, 'ache', 'bug');
// a is ['a', 'ache', 'bug', 'c']
// r is ['b']
//Delete the element index=3
array.splice(index, 1);
```

* **UTILS**

ND **`array.findIndex()`** - returns the index of the first element in the array that satisfies the provided testing function. Otherwise -1 is returned.  

```js
const arr = [5, 12, 8, 130, 44];

function findFirstLargeNumber(element) {
  return element > 13;
}

console.log(arr.findIndex(findFirstLargeNumber)); // 3
```

### Objects

* **Object.keys(obj)** returns an array whose elements are strings corresponding to the enumerable properties found directly upon object

```js
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']
```

```js
let days = {
  '2017-05-20': {
    users: ['user1', 'user2'],
    hits: '4'
  },
  '2017-05-21': {
    users: ['user3'],
    hits: '5'
  }
}
// convert users[] to users.length
Object.keys(days).map(function (index) {
  if (Array.isArray(days[index].users)) {
    days[index].users = days[index].users.length;
  }
});
```

---

## DATA STRUCTS


```js
let dayHits = {
  'day1': {
    users: ['user1', 'user2'],
    hits: '4'
  },
  'day2': {
    users: ['user3'],
    hits: '5'
  }
}

let countries = {
  'US': { 
    'users' : 1,
  }
}

// create data struct
let start = new Date(lastSaved);
let end = new Date();
while (start <= end) {
  const dayDate = start.toISOString().split('T')[0];
  days[dayDate] = { // we create key using []
    'hits': 0,
    'users': []
  };
  start = new Date(start.setDate(start.getDate() + 1));
}
```


```js 
let dayServices = {
  'service1': {
    users: ['user1', 'user11'],
    hits: '1'
  },
  'service2': {
    users: ['user2', 'user22'],
    hits: '2'
  }
}

let dayHits = {
  'day1': dayServices,
  'day2': dayServices
}

console.log(dayHits['day2']['service2'].users)
```

---

## CONVERSION

### Object to Array

```js
const createEvents = () => {
  let controls = document.getElementsByClassName('control');
  // both are valid conversions
  controls = Array.from(controls)
  controls = Object.values(controls);
  // now i can iuse forEach
  controls.forEach(function (element, i) {
    console.log(i, element);
  });
};
```

---

## STANDARD LIBRARY

### Math

* **PROPERTIES**

`Math.PI` - Returns PI (3.14159265359)    
`Math.E`- Euler number (2.718)   

* **METHODS**

`Math.abs(num)` - Returns the absolute value of x  
`Math.round(num)` - Rounds x to the nearest integer  
`Math.sqrt(num)` - Returns the square root of x   
`Math.ceil(num)` - Returns x, rounded upwards to the nearest integer  
`Math.floor(num)` - Returns x, rounded downwards to the nearest integer  
`Math.random()` - Returns a random number between 0 and 1  
`Math.pow(x, y)` - Returns the value of x to the power of y  
`Math.max(num1, num2, num3)` - Returns the number with the highest value  
`Math.min(num1, num2, num3)` - Returns the number with the lowest value  

`Math.acos(x)` - Returns the arccosine of x, in radians  
`Math.asin(x)` - Returns the arcsine of x, in radians    
`Math.atan(x)` - Returns the arctangent of x as a numeric value between -PI/2 and PI/2 radians   
`Math.cos(x)` - Returns the cosine of x (x is in radians)  
`Math.sin(x)` - Returns the sine of x (x is in radians)  
`Math.tan(x)` - Returns the tangent of an angle   

### Date

* **CREATE DATE OBJECT**

```javascript
var d = new Date();
var d = new Date(millis);
var d = new Date(dateString);
var d = new Date(year, month, day, hours, minutes, seconds, millis); 
```

* **METHODS**

`Date.now()` - Returns the number of milliseconds since midnight Jan 1, 1970    
`Date.parse()` - Parses a date string and returns the number of milliseconds since January 1, 1970

`.getDay()` - day of the week (from 0-6)  
`.getDate() .setDate()` - day of the month (from 1-31)    
`.getFullYear() .setFullYear()` - year (4 digits)    
`.getHours() .setHours()` - Hour (0-23)  
`.getMilliseconds() .setMilliseconds()` - Milliseconds (0-999)  
`.getMinutes() .setMinutes()` - Minutes (0-59)  
`.getMonth() .setMonth()` - Month (0-11)  
`.getSeconds() .setSeconds()` - Seconds (0-59)  
`.getTime() .setTime()` - milliseconds since midnight Jan 1, 1970.  
`.getTimezoneOffset()` - Returns the time difference between UTC time and local time, in minutes  

UTC  
`.getUTCDate()` , `.setUTCDate()` - Dia del mes (1-31)  
... add UTC

`.toDateString()` - Converts Date object into a readable string "Mon Jan 17 1982"  
`.toTimeString()` - Converts Date object into a readable string "19:45:15 GMT+0300 (CEST)"    
`.toString()` - Converts a Date object to a string    
`.toISOString()` -  Returns the date as a string, using the ISO standard  
`.toJSON()` - Returns the date as a string, formatted as a JSON date.    
`.toLocaleDateString()` - Returns the date portion of a Date object as a string, using locale conventions   
`.toLocaleTimeString()` - Returns the time portion of a Date object as a string, using locale conventions  
`.toLocaleString()` - Converts a Date object to a string, using locale conventions  
`.toUTCString()` - Converts a Date object to a string, according to universal time  
`.valueOf()` - Returns the primitive value of a Date object   

* **UTILS**

Add one day
```js
let start = new Date();
start.setDate(start.getDate() + 1); // add one day
```


---

