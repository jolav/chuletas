# NODEJS 4

---

## NODEJS

### arquitectura asincrona

__Eventos sincronos__ con Bloqueo de I/O y multiples hilos

![nodejs](/z-static/images/nodejs/sincrono.png)

__Evento sincrono demultiplexado__ Sin bloqueo de I/O y un unico hilo. Las
tareas se reparten en el tiempo.

![nodejs](/z-static/images/nodejs/demultiplexado.png)

__Evento Asincrono__ la aplicacion expresa el interes de acceder a un recurso
en un momento dado ( sin bloquear I/O) y provee un manejador que sera invocado
en otro momento cuando la operacion de acceso al recurso termine

![nodejs](/z-static/images/nodejs/reactorPattern.png)

### callbacks

> * Son los manejadores del patron del reactor.
> * Son funciones invocadas para propagar el resultado de una operacion.
> * Practicamente sustituyen al return que es sincrono.

* Estilo directo sincrono

```javascript
function add(a, b) {
  return a + b;
}
```

* Continuation passing style - CPS sincrono

```javascript
function add(a, b, callback) {      // add es una funcion sincrona
  callback (a + b);
}
console.log('antes');
add (1, 2, function(result) {
  console.log('resultado: ' + result);
});
console.log('despues');

RESULTADO
antes
resultado: 3
despues
```

* Continuation passing style - CPS asincrono

```javascript
function addAsync(a, b, callback) {
  setTimeout(function() {
    callback(a + b);
  }, 100);
}
console.log('antes');
addAsync(1, 2, function(result) {
  console.log('resultado: ' + result);
});
console.log('despues');

RESULTADO
antes
despues
resultado: 3
```

![nodejs](/z-static/images/nodejs/asincrono.png)

* Non continuation passing style - CPS callbacks

```javascript
var result = [1, 5, 7].map(function(element) {
  return element – 1;
});
Claramente el callback solo se usa para iterar los elementos del array
y no para pasar el resultado de la operacion
No siempre la presencia de callbacks implica estilo asincrono o CPS.
Hay que leer la documentacion de las API para conocerlo
```

__Convenciones sobre los callbacks en node.js :__

1. Los callbacks al final, son el ultimo argumento

```javascript
fs.readFile(filename, [options], callback);s
```

2. Los errores primero

```javascript
fs.readFile('foo.txt', 'utf8', function(err, data) {
  if(err) {
    handleError(err);
  } else }
    processData(data);
  }
});
```

3. Propagacion de errores, en sincrono se usa el comando throw.
Pero en asincrono CPS se hace pasando el error al siguiente callback

```javascript
var fs = require('fs');
function readJSON(filename, callback) {
  fs.readFile(filename, 'utf8', function(err, data) {
    var parsed;
    if(err)
      return callback(err) //propagate error and exit the current function
    try {
      parsed = JSON.parse(da);     //parse the file contents
    } catch(err) {
      return callback(err); //catch parsing errors
    }
    callback(null, parsed);        //no errors, propagate just the data
  });
};
```

4. Uncaught exceptions, se realizan con el try-catch visto en el ejemplo justo
arriba

### clase EventEmitter

* **Observer Pattern (Patron del observador)** define un objeto llamado .
sujeto que puede notificar a un conjunto de observadores (u oyentes) cuando ha
ocurrido un cambio en su estado  
Esto se consigue en nodejs a traves de la clase `EventEmitter`

```javascript
var EventEmitter = require('events').EventEmitter;
var eeInstance = new EventEmitter();
```

* **Metodos de EventEmitter**   
Los `listener` son funciones que se ejecutan al realizarse el evento

```javascript
on(event, listener);
once(event, listener);
emit(event, [arg1], [...]);
removeListener(event, listener);
```

```js
var events = require('events');
var eventEmitter = new events.EventEmitter();

var connectHandler = function connected() {
   console.log('connection succesful.');
   eventEmitter.emit('data_received');
}
eventEmitter.on('connection', connectHandler);
eventEmitter.on('data_received', function(){
   console.log('data received succesfully.');
});
eventEmitter.emit('connection');
console.log("Program Ended.");
```

---

## NPM

`npm list -g --depth=0` Paquetes instalados globalmente  
`npm list --depth=0` Paquetes instalados localmente  
`npm view paquete version` Version disponible mas moderna  
`npm install paquete -g` Instala paquete global  
`npm install paquete` Instala paquete local. Se usara require luego en la
app para usarlo  
`npm uninstall paquete -g` Desintala paquete global  
`npm uninstall paquete` Desinstala paquete local  
`npm update -g` Actualizar paquetes globales  
`npm update` Actualizar paquetes locales  
`npm install paquete@0.0.0` Instalar version especifica de un paquete  

Actualizar paquetes a la ultima version estable

`npm outdated` en el directorio local donde package.json y nos indica como estan los paquetes, si estan todos actualizados no sale ningun resultado  
`npm update` en el directorio donde esta package.json actualiza los paquetes  

```sh
npm install -g npm-check-updates
npm-check-updates -u
npm install
```

`Semver` versiones semanticas
![nodejs](/z-static/images/nodejs/semver.png)

### nvm

1. Es trivial de desinstalar  
2. No necesita permisos de administrador para instalarse  
3. Permite tener varias versiones de node facilmente  


---

## FILE SYSTEM

[File System](https://nodejs.org/dist/latest-v4.x/docs/api/fs.html)

`fs.appendFile(file, data[, options], callback)` - añadir datos al final del
fichero   
`fs.appendFileSync(file, data[, options])` - añadir datos al final del
fichero de forma sincrona  
`fs.close(fd, callback)` - cierra un archivo  
`fs.closeSync(fd)` - cierra un archivo de forma sincrona  
`fs.createReadStream(path[, options])` - crea un nuevo objeto ReadStream  
`fs.createWriteStream(path[, options])` - crea un nuevo objeto WriteStream  
`fs.mkdir(path[, mode], callback)` - crea un directorio  
`fs.mkdirSync(path[, mode])` - crea un directorio de forma sincrona  
`fs.open(path, flags[, mode], callback)` - abre un archivo  
`fs.openSync(path, flags[, mode])` - abre un archivo de forma sincrona  
`fs.readdir(path, callback)` - leer un directorio  
`fs.readdirSync(path)` - leer un directorio de forma sincrona  
`fs.readFile(file[, options], callback)` - leer el contenido entero de un archivo  
`fs.readFileSync(file[, options])` - leer el contenido entero de un archivo de
forma sincrona  
`fs.read(fd, buffer, offset, length, position, callback)` - lee datos de un archivo  
`fs.readSync(fd, buffer, offset, length, position)` - lee datos de un archivo de
forma sincrona   
`fs.rename(oldPath, newPath, callback)` - cambia el nombre o ubicacion de un
archivo   
`fs.renameSync(oldPath, newPath)` - cambia el nombre o ubicacion de un archivo de forma sincrona   
`fs.rmdir(path, callback)` - borra un directorio  
`fs.rmdirSync(path)` - borra un directorio de forma sincrona  
`fs.stat(path, callback)` - consigue el estado de un archivo  
`fs.statSync(path)` -  consigue el estado de un archivo de forma sincrona  
`fs.unlink(path, callback)` - borra un archivo  
`fs.unlinkSync(path)` - borra un archivo de forma sincrona  
`fs.unwatchFile(filename[, listener])` - deja de vigilar cuando se producen cambios
en un archivo  
`fs.watch(filename[, options][, listener])` - vigila cuando se producen cambios en
un archivo o directorio  
`fs.watchFile(filename[, options], listener)` - vigila cuando se producen cambios
en un archivo   
`fs.write(fd, buffer, offset, length[, position], callback)` - escribe el buffer al
archivo  
`fs.writeSync(fd, buffer, offset, length[, position])` - escribe el buffer al
archivo de forma sincrona  
`fs.write(fd, data[, position[, encoding]], callback)` - escribe los data al
archivo  
`fs.writeSync(fd, data[, position[, encoding]])` - escribe los data al archivo de
forma sincrona   
`fs.writeFile(file, data[, options], callback)` - escribe data al archivo    
`fs.writeFileSync(file, data[, options])` - escribe data al archivo de forma
sincrona  
`fs.stat(path, callback)` - consigue informacion del archivo  

---

## GLOBAL OBJECTS

[Global objects](https://nodejs.org/dist/latest-v4.x/docs/api/globals.html)  
Disponibles en todos los modulos  


`Class: Buffer` - para manejar datos binarios  
`__dirname` - directorio actual  
`__filename` - nombre del archivo que se esta ejecutando  
`console` - para imprimir datos  
`exports` - una referencia a `module.exports`, no es global sino local a cada
modulo  
`module` - una referencia al modulo actual, no es global sino local a cada
modulo  
`process` - objeto proceso    
`require()` - para usar modulos  
`setImmediate(cb[, arg][, ...])` - ejecuta el callback inmediatamente  
`setInterval(cb, delay[, arg][, ...])` - ejecuta el callback cada delay
milisegundos  
`setTimeout(cb, delay[, arg][, ...])` - ejecuta el callback una sola vez despues
de delay milisegundos  

---

## MODULOS

[Libro](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript)

### object literal

```js
var myModule = {
  myProperty: "someValue",
  myConfig: {
    useCaching: true,
    language: "en"
  },
  saySomething: function () {
    console.log( "Where is Paul Irish today?" );
  },
  reportMyConfig: function () {
    console.log( "Caching is: " +
        ( this.myConfig.useCaching ? "enabled" : "disabled") );
  },
  updateMyConfig: function( newConfig ) {
     if ( typeof newConfig === "object" ) {
      this.myConfig = newConfig;
      console.log( this.myConfig.language );
    }
  }
};
myModule.saySomething();                // Where is Paul Irish today?
myModule.reportMyConfig();              // Caching is: enabled
myModule.updateMyConfig({               // fr
  language: "fr",
  useCaching: false
});
myModule.reportMyConfig();              // Caching is: disabled
```

### module pattern

Se usa para replicar el concepto de clases para guardar metodos y variables
publicas y privadas dentro de un objeto.  
Eso nos permite crear una API publica para los metodos que queramos  

* **Closures anonimos**

```js
(function () {
	// ... all vars and functions are in this scope only
	// still maintains access to all globals
}());
```

* **Global Import**

```js
(function ($, YAHOO) {
	// now have access to globals jQuery (as $) and YAHOO in this code
}(jQuery, YAHOO));
```

* **Module Export**

```js
var Modulo = (function () {
  var mod = {};
  var privadaVariable = 1;
  function privadoMetodo () {
    return privadaVariable++;
  }
  mod.moduloPropiedad = 'Hola';
  mod.moduloMetodo = function () {
    return privadoMetodo();
  };
  return mod;
})();
console.log(Modulo.moduloPropiedad);                  // Hola
console.log(Modulo.moduloMetodo());                   // 1
console.log(Modulo.moduloMetodo());                   // 2
```

* **Object Interface**

```js
var weekDay = function() {
  var names = ["Sunday", "Monday", "Tuesday", "Wednesday","Thursday",
                                              "Friday", "Saturday"];
  return {
    name: function(number) {
      return names[number];
    },
    number: function(name) {
      return names.indexOf(name);
    }
  };
}();
console.log(weekDay.name(weekDay.number("Sunday")));    // → Sunday
```


* **Revealing module pattern**

Autoejecutable

```js
var myRevealingModule = (function () {
  var privateVar = "Ben Cherry";
  var publicVar = "Hey there!";
  function privateFunction() {
    console.log( "Name:" + privateVar );
  }
  function publicSetName( strName ) {
    privateVar = strName;
  }
  function publicGetName() {
    privateFunction();
  }
  // Reveal public pointers to
  // private functions and properties
  return {
    setName: publicSetName,
    greeting: publicVar,
    getName: publicGetName
  };
})();
myRevealingModule.setName( "Paul Kinlan" );
```

Con creacion manual de objetos

```js
function User(){
  var username, password;
  function doLogin(user,pw) {
    username = user;
    password = pw;
  }
  var publicAPI = {
    login: doLogin
  };
  return publicAPI;
}
var fred = User();                              // crea un modulo User  
fred.login("fred", "Battery34");
```

### commonJS

Un modulo commonJS es una codigo reusable que exporta objetos especificos que
se pueden usar en otros modulos con `require` como se hace en nodejs.  

```js
function myModule() {
  this.hello = function() {
    return 'hello!';
  }
  this.goodbye = function() {
    return 'goodbye!';
  }
}
module.exports = myModule;
```

```js
var myModule = require('myModule');
var myModuleInstance = new myModule();
myModuleInstance.hello(); // 'hello!'
myModuleInstance.goodbye(); // 'goodbye!'
```

CommonJS carga los modulos de forma sincrona.  
Es perfecto para el servidor (los archivos desde el disco se leen muy rapidos)
pero para el cliente no (leer modulos a traves de internet puede ser lento) y
mientras lee bloquea el navegador para que no haga nada mas.

Con [browserify](http://browserify.org/) podemos usar `require("modulo")` en el
navegador  

### AMD

`AMD definicion asincrona de modulos` Envolver el codigo del modulo en una
funcion `define` para que se llame al modulo cuando ya se han cargado las
dependencias.  

Los modulos deben crearse usando `define`  

```js
define([], function() {
  return {
    hello: function() {
      console.log('hello');
    },
    goodbye: function() {
      console.log('goodbye');
    }
  };
});
```

`define([array], callback)`  
> `array` - el primer argumento es un array de dependencias necesarias que se cargan en segundo  plano sin bloquear nada (p ej el navegador). Cuando se
cargan se ejecuta el callback   
> `callback` - el segundo argumento es una funcion callback que tiene por  argumentos los modulos que se han cargado y permite ya el uso de los mismos    

```js
define(['module', 'otherModule'], function(module, otherModule) {
  console.log(module.hello());
});
```

otro ejemplo

```js
//    filename: foo.js
define(['jquery'], function ($) {
    //    methods
    function myFunc(){};
    //    exposed public methods
    return myFunc;
});
```

No puede usarse en el servidor  
Con [requirejs](http://requirejs.org/) podemos usarlo en el servidor  

### UMD

Soporta commonJS y AMD por lo que se puede usar en cliente y servidor   

### ES6

**INCOMPLETO**

Se pueden usar en servidor y cliente.  
Cargan de forma asincrona  

```js
// lib/counter.js
var counter = 1;
function increment() {
  counter++;
}
function decrement() {
  counter--;
}
module.exports = {
  counter: counter,
  increment: increment,
  decrement: decrement
};

// src/main.js
var counter = require('../../lib/counter');
counter.increment();
console.log(counter.counter); // 1
```

```JSON
/* index.html */
<script type='module' src='./app.js'>

/* app.js */
import { sum } from './math.js';
console.log(sum(1, 2));

/* math.js */
export function sum(a, b) { return a + b; }
export function mult(a, b) { return a * b; }
```

---

## TESTING

![testing](/z-static/images/testing/testing.png)

Mocha vs Tape

---

### tdd

`test driven development` - primero los test y luego el codigo  

---





