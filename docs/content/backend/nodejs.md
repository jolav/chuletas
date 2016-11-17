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

## EXPRESS 4.14

### instalacion

`npm install -g express`  - Lo instalamos globalmente  
`npm install -g express-generator` - Y el generador de codigo plantilla que nos permite usar el comando express  

* A pelo  
`npm init` - te pregunta muchas cosas para generar el `package.json`  

* Con plantillas  
`express -h` Muestra las opciones  
`express --hbs miApp` Crea el proyecto miApp con handlebars pej  
`cd miApp & npm install` Instala las dependencias  

![express](/z-static/images/express/fileStructure.png)
![express](/z-static/images/express/fileStructure2.png)


### api

* `application app` Es una instancia de la aplicacion express que se usa
normalmente para configurar la aplicacion
* `request req` Es una envoltura del objeto request del modulo HTTP usado
para extraer informacion sobre la peticion
* `response res` Es una envoltura del objeto response del modulo HTTP usado
para enviar los datos y cabeceras de la respuesta

**Application settings:**

* env: production, development ...
* viewcache
* view engine: jade, ejs, hbs ...
* views
* trustproxy (NGINX)

![express](/z-static/images/express/expressChuleta.png)


**Seguimiento de una peticion en express**

![express](/z-static/images/express/requestExpress.png)

### CORS

Antes de las rutas añadir las cabeceras  

```js
app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});
```

## MAIN FILE

**`app.js` o `main.js`** Cosas que se hacen en este fichero:

1. `Dependencias` - Incluir paquetes de terceros y modulos nuestros como
 manejadores, utilidades ayudantes y modelos
2. `Instanciaciones` - configurar las opciones de express como motor de plantillas
3. `Configuraciones` - ajustes y como conectar las bases de datos
4. `Middlewares` - Definir middlewares
5. `Ruteo` - definir rutas
6. `Arranque` - iniciar la app o exportarla app como un modulo

### ajustes

`app.set(nombre, valor)` - para definir un valor

```js
app.set('port', process.env.PORT || 3000);
```

`app.get(nombre)` - para conseguir un valor  
`app.enable()` -   
`app.disable()` -  
`app.enabled()` -   
`app.disabled()` -  
`env` - almacena el modo de entorno actual para ese proceso de node.js. Se toma
de `process.env.NODE_ENV` o se pone `development` por defecto. Opciones
{development, test, stage, preview, production}  
`view cache` - ponerlo a `true` para produccion  
`view engine` - [view engine](#motor-de-plantillas)  
`views` -  la ruta absoluta al directorio con las plantillas  

```js
 app.set('views', path.join(__dirname, 'templates'))
```

`trust proxy` - ponerlo a true se esta detras de un proxy como nginx. Por
defecto esta desactivado, para activarlo  

```js
app.set('trust proxy', true);
app.enable('trust proxy');
```

`jsonp callback name` - vale para hacer llamadas ajax desde otros dominios
usando CORS  

```js
app.set('jsonp callback name', 'cb');
```

`json replacer` - `json spaces` - son dos parametros que se aplican a todas las
funciones JSON.stringify().   
replacer es un filtro  
spaces es valor de indentacion  

```js
app.set('json replacer', function(key, value){
  if (key === 'discount')
    return undefined;
  else
    return value;
});
app.set('json spaces', 4);
```

`case sensitive routing` - por defecto disabled, asi /nombre y /Nombres es la
misma ruta  
`strict routing` - por defeto disabled, asi /users y /users/ son la misma ruta  
`x-powered-by` - establece la opcion de la respuesta `x-powered-by` con el
valor Express. Por seguridad lo mejor es desactivarla  

```js
app.set('x-powered-by', false) // o
app.disable('x-powered-by')
```

`etag` - dejarlo como esta es lo mejor  
`query parser` - para parsear las `query-string` enviadas en la URL. Por defecto
extended (usa el modulo qs), true (qs), false (lo desactiva), simple (usa el
modulo del nucleo querystring)  

```js
app.set('query parser', 'simple');
app.set('query parser', false);
app.set('query parser', customQueryParsingFunction);
```

`subdomain offset` - Para aplicaciones desplegadas en varios subdominios  

### contenido estatico

* **Usar directorio de contenido estatico**

Para servir contenido estatico se usa el middleware `express.static` incorporado
en express.  

```js
var publicPath = path.resolve(__dirname, 'public');
app.use(express.static(publicPath));
```

Ya puedes cargar archivos del directorio `public`  
El nombre del directorio `public` no es parte de la url  
`http://localhost:3000/css/style.css`   
`http://localhost:3000/public/css/style.css` NO   

* **Usar multiples directorios de contenido estatico**   

Express busca los archivos en el orden en el que se declaran los directorios
estaticos  

```js
app.use(express.static('public'));
app.use(express.static('files'));
```

* **Prefijo de ruta virtual**

```js
app.use('/static', express.static('public'));
```

Ahora para cargar los archivos :
`http://localhost:3000/static/css/style.css`

---

## TEMPLATES

[Handlebars con express.js](/backend/nodejs/#handlebars)

---

## REQUEST

Es un envoltorio para el objeto `http.request` del modulo del core  

`req.query` - objeto con los parametros de la querystring (parametros GET) como parejas  
nombre/valor  
`req.params` - array que tiene los parametros de la ruta con nonbre  
`req.body` - objeto con los parametros POST  
`req.route` - informacion sobre la ruta actual  
`req.cookies` - `req.signedCookies` - objetos con valores de cookies suministrados
por el cliente  
`req.headers` - las cabeceras de la peticion recibidas del cliente   
`req.ip` - la ip del cliente    
`req.xhr` - devuelve true si la peticion es una llamada AJAX    

---

## RESPONSE

`res.render(view, [locals], callback)` - renderiza una vista usado el motor de plantillas configurado  
`res.locals` -  
`res.set(name, value)` - establece una cabecera de la respuesta    
`res.status(code)` - establece el codigo de estado http    
`res.send([status], body)` - envia una respuesta al cliente    
`res.json([status], json)` - envia json al cliente    
`res.jsonp([status], jsonp)` - envia jsonp al cliente  
`res.redirect([status], url)` - redirige el navegador  

---

## MIDDLEWARE

### que son

* Se usan para gestionar las peticiones HTTP hechas al servidor, manejarlas y responderlas.  
* Son callbacks que se ejecutan cuando ocurre una peticion HTTP.  
* La mayoria son de diseño propio para cubrir nuestras necesidades pero tambien
los hay para cosas comunes como logueos, servir ficheros estaticos, parseos y
muchas mas cosas.  
* `orden` - Los middlewares se ejecutan en el mismo orden en el que se cargan  


**Que puede hacer un middleware**

* ejecutar codigo  
* hacer cambios en los objetos request y response  
* terminar el ciclo request-response  
* llamar al siguiente middleware  

Si el middleware en curso no termina el ciclo request-response debe llamar a
`next()` para pasar el control al siguiente middleware o la peticion se quedara
colgada.  

**Cada funcion middleware tiene tres argumentos:**

1. `req`: objeto que contiene toda la informacion de la peticion HTTP
2. `res`: objeto que contiene toda la informacion de la respuesta HTTP
3. `next`: es el siguiente middleware definido en el orden de middlewares

### como se usan

**Crear un middleware**

```js
var express = require('express');
var app = express();

// Lo definimos
var myLogger = function (req, res, next) {
  console.log('LOGGED');
  next();
};

// Lo cargamos en la app antes de definir las rutas o nunca se ejecutara
app.use(myLogger);

app.get('/', function (req, res) {
  res.send('Hello World!');
});

app.listen(3000);
```

Otro ejemplo middleware pero este manipula la peticion

```js
var express = require('express');
var app = express();

// Lo definimos
var requestTime = function (req, res, next) {
  req.requestTime = Date.now();
  next();
};

// Lo cargamos en la app antes de definir las rutas o nunca se ejecutara
app.use(requestTime);

app.get('/', function (req, res) {
  var responseText = 'Hello World!<br>';
  responseText += '<small>Requested at: ' + req.requestTime + '</small>';
  res.send(responseText);
});

app.listen(3000);
```

Limitar un middleware a una cierta rutas

```js
app.use(/ruta, middlewareParaEsaRuta);
```

**Para cada ciclo request-response de una aplicacion Express**  

* Se ejecutan los middlewares de arriba a abajo
* Para no terminar el ciclo un middleware debe pasar el control al siguiente
mediante un next();
* next('route'); se salta el resto de middlewares del stack de rutas  
* Para terminar un ciclo debemos usar `res.end` o usar `res.send() ó
res.sendFile()` que internamente llaman a `res.end`  

### tipos

**Tipos**

`de aplicacion` - atados a una instancia de express  

```js
var app = express();
app.use() ó app.METHOD() [METHOD = GET, PUT, POST, UPDATE ...]
```
`de ruta` - atados a una instancia de express.Router()  

```js
var router = express.Router();
router.use() ó router.METHOD() [METHOD = GET, PUT, POST, UPDATE ...]
```

`de error` - tienen un argumento mas (err, req, res, next)  

```js
app.use(function(err, req, res, next) {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

`de builtin` - los antiguos de connect, solo queda [`express.static`](#contenido-estatico)  

`de terceras-partes` - antes de usarlos hay que instalarlos  

```js
npm install middleware;
var middleware = require('middleware');
app.use(middleware());
```

**Esenciales**

`compression` - gzips los datos transferidos. Debe ponerse my arriba para que
comprima los datos de otros middlewares y rutas  

```js
npm install compression
var compression = require('compression');
app.use(compression());
```

`express-static`  

[contenido estatico](#contenido-estatico)

`morgan` - antiguo logger. Lleva registro de todas las peticiones y otra
informacion importante  

```js
npm install morgan
var logger = require('morgan');
app.use(logger("common | dev | short | tiny | y mas"));  // usar dev
```

`connect-timeout` - establece un temporizador

```js
npm install connect-timeout
var timeout = require('connect-timeout');
```

`body-parser` - permite procesar los datos que vienen y convertirlos en objetos
javascript/node usables.  

```js
npm install body-parser
var bodyParser = require('body-parser');
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
```

`errorhandler` - se usa para manejo basico de errores en desarrollo y
prototipado

```js
npm install errorhandler
var errorHandler = require('errorhandler');
if (app.get('env') === 'development') {
  app.use(errorhandler());
}
```

`connect-busboy` - para usar el parseador de formularios `busboy`

```js
npm install connect-busboy
var busboy = require('connect-busboy');
app.use('/upload', busboy({immediate: true }));
```

`cookie-parser` - permite acceder los valores de las cookies del usuario del
objeto `req.cookie`

```js
npm install cookie-parser
var cookieParser = require('cookie-parser');
app.use(cookieParser());
```

`method-override` - permite al servidor soportar metodos http que el cliente no
soporte.   

```js
npm install method-override
var methodOverride = require('method-override');
app.use(methodOverride("loQueToque"));
```

`serve-index` - como un `ls` en la terminal   

```js
npm install serve-index
var serveIndex = require('serve-index');
app.use('/shared', serveIndex(
  path.join('public','shared'),
  {'icons': true}
));
```

`express-session` - permite al servidor usar sesiones web. Necesita tener activo
antes a cookie-parser.

```js
npm install express-session
```

`response-time` - añade la cabecera "X-Response-Time" con un tiempo en ms con un numero de 3 digitos por defecto desde el momento en el que la peticion entro en este middleware.

```js
npm install response-time
var responseTime = require('response-time');
app.use(responseTime(4));    // cambio a 4 digitos
```

`csurf` - para prevenir CSRF

```js
npm install csurf
var csrf = require('csurf');
app.use(csrf());
```

`serve-favicon` - para configurar el favicon que querramos  

```js
npm install serve-favicon
var favicon = require('serve-favicon');
app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(favicon(path.join(__dirname + '/public', 'favicon.ico')));
```

`vhost` - para usar diferentes rutas basadas en dominios. Por ejemplo tenemos
dos apps con express (api y web) para organizar el codigo para diferentes
rutas basadas en los dominios api.dominio.com y web.dominio.com  

```js
npm install vhost
var api = express()
var web = express()
app.use(vhost('www.dominio.com', web))
app.use(vhost('api.dominio.com', api))
```

`cookie-session` - almacen de sesion de cookies  
`raw-body` - para peticiones como buffers  
`express-validator` - para sanear y validar datos que se reciben  
`passport` - autenticacion  
`oauth2-server` - autenticacion  
`helmet` - middlewares de seguridad  
`cors` - soporte cors para express  
`connect-redis` - almacen de la sesion en redis  

---

## ROUTING

El ruteo determina como responde una aplicacion a una peticion de un cliente que
contiene una URI (o ruta) y un metodo HTTP (GET, POST ...)  
Cada ruta puede tener una o mas funciones manejadoras que se ejecutan cuando la
ruta coincide.  
Las rutas se pueden hacer con expresiones regulares  

```sh
protocolo  hostname       port    path       querystring     fragment

http://    localhost      :3000   /about     ?test=1         #history
http://    www.bing.com           /search    ?q=grunt&first
https://   google.com             /                          #q=express
```

### app.METHOD

**`app.METHOD(path, [handler], handler);`**  

* `path` es la ruta en el servidor que pueden ser cadenas, patrones de
cadenas o expresiones regulares  
* `handler` es la funcion que se ejecuta cuando la ruta coincide. Cuando hay varios manejadores para evitar que se ejecuten todos hay que invocar next('route') y
saltara a la siguiente  
* `METHOD`  
    - get, leer  
    - post, crear  
    - put, actualizar  
    - delete, borrar  
    - hay otros cuantos  
    - all, para todos  

```js
// Ejemplos
app.get('/', function (req, res) {              // GET method route
  res.send('GET request to the homepage');
});
app.post('/', function (req, res) {             // POST method route
  res.send('POST request to the homepage');
});
```

**parametros en la ruta**

La ruta puede contener parametros que se capturan en `req.params`

```json
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

**multiples manejadores de ruta**

Se puede establecer múltiples callbacks (manejadores) que se comportan como middlewares para gestionar una petición.  
La unica excepcion es que podrian invocar `next("route")` para evitar los
restantes manejadores de esa ruta  
Los manejadores de ruta pueden ser una funcion, un array de funciones o una
combinacion de ambos  

```js
// varias funciones independientes
app.get('/example/b', function (req, res, next) {
  console.log('the response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from B!');
});
```

```js
// array de funciones
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}
var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}
var cb2 = function (req, res) {
  res.send('Hello from C!');
}
app.get('/example/c', [cb0, cb1, cb2]);
```

```js
// mix de funciones independientes y array de funciones
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}
var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}
app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('the response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from D!');
});
```

### app.route()

cadena de manejadores de ruta

```js
app.route('/book')
 .get(function(req, res) {
   res.send('Get a random book');
 })
 .post(function(req, res) {
   res.send('Add a book');
 })
 .put(function(req, res) {
   res.send('Update the book');
 });
```

### clase express.Router

La clase `express.Router` se usa para crear manejadores modulares

Creamos un archivo perfil.js en el subdirectorio para rutas /routes  

```js
var express = require('express');
var perfil = express.Router();

// middleware especifico para esta ruta si nos hace falta, es opcional
perfil.use (function(req, res, next) {
  console.log('Hora = ', Date.now());
  next();
});
perfil.get("/", function(req, res, next) {
  res.send("En ruta /");
});
perfil.get("/username", function(req, res, next) {
  res.send("En /username");
});
module.exports = perfil;
```

Cargamos el modulo de router en la aplicacion en app.js

```js
// en app.js con esto creamos dos rutas "/profile" y "/profile/:username"
var perfiles = require('./routes/perfil.js');
app.use('/perfiles', perfiles);
```

Ahora la app ya atiende a las rutas `/perfil` y `/perfil/:username`  

Cada router puede cargar otros router hasta cargar todos en pej index.js y al
cargar en app,js /controller/index.js ya estaran todas las rutas  

---

## ERRORES

El middleware de manejo de errores se define el ultimo despues de todos los
app.use() y manejo de rutas.  

* En desarrollo

```js
if (app.get('env') === 'development') {
  app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
      message: err.message,
      error: err
    });
  });
}
```

* En produccion

```js
app.use(function(err, req, res, next) {
  res.status(err.status || 500);
  res.render('error', {
    message: err.message,
    error: {}
  });
});
```

![express](/z-static/images/express/codigosEstado.png)

Se produce un error en express cuando llamamos un `next("algo ha pasado")`   
Se saltara los middleware que haya hasta llegar al primer middleware que maneje errores  
Express no maneja errores producidos por la palabra clave `throw` y tiene
protecciones para estas excepciones. La app retirnara un error 500 y esa peticion
fallara pero la app continuara ejecutandose. Sin embargo los errores de sintaxis
cascan el programa  

```js
app.use(function(req, res, next) {
  if (req.url === "/") {
    next();
  } else if (req.url === "/throw") {
    throw new Error("Gimme that error");
  } else {
    next("You didn't visit the root!");
  }
});

app.use(function(req, res) {
  res.send("Welcome to the homepage.");
});

app.use(function(err, req, res, next) {
  console.error(err);
  res.status(500);
  next(err);
});

app.use(function(err, req, res, next) {
  res.send("Got an error: " + err);
});
```

---

## APIs

[RESTful API](http://www.tutorialspoint.com/nodejs/nodejs_restful_api.htm)

![express](/z-static/images/express/traditional.png)
![express](/z-static/images/express/restApiAjax.png)

* **CRUD api**

`crear -> POST`  
`leer -> GET`  
`actualizar -> PUT`  
`borrar -> DELETE`  

* **Versiones**

Patron para mantener la retrocompatibilidad

`api1.js`  

```js
var express = require("express");
var api = express.Router();
api.get("/timezone", function(req, res) {
  res.send("Sample response for /timezone");
});
api.get("/all_timezones", function(req, res) {
  res.send("Sample response for /all_timezones");
});
module.exports = api;
```

`api2.js`  

```js
var express = require("express");
var api = express.Router();
api.get("/timezone", function(req, res) {
  res.send("API 2: super cool new response for /timezone");
});
module.exports = api;
```

`app.js`  

```js
var express = require("express");
var apiVersion1 = require("./api1.js");
var apiVersion2 = require("./api2.js");
var app = express();
app.use("/v1", apiVersion1);
app.use("/v2", apiVersion2);
app.listen(3000, function() {
  console.log("App started on port 3000");
});
```

---

## SEGURIDAD

[Seguridad en express](http://expressjs.com/en/advanced/best-practice-security.html)

* **Cross-site request forgery**

`npm install --save csurf` - Se evita al usar el middleware csurf  
Debe ir precedido por `cookie-parser` y `express-session`  

```js
var cookieParser = require('cookie-parser'),
var session = require('express-session');
var csrf = require("csurf");

app.use(cookieParser('FE28D342-4040-4D0E-B080-B85E85DAF7FD'));
app.use(session({
  secret: 'BD564488-5105-4202-8927-5A5C9AE9154E',
  resave: true,
  saveUninitialized: true
}));
app.use(csrf());
app.use(function (request, response, next) {
  response.locals.csrftoken = request.csrfToken();
  next();
});
```

* **Permisos en procesos**

No correr servicios como root.  
Los servicios se pueden cambiar de usuario en ejecucion usando GID (group ID) y
UID (user ID)  

```js
var app = express();
http.createServer(app).listen(app.get('port'), function(){
  console.log("Express server listening on port " + app.get('port'));
  process.setgid(parseInt(process.env.GID, 10));
  process.setuid(parseInt(process.env.UID, 10));
});
```

* **HTTP Security Headess**

Instalar middleware `helmet` que es un conjunto de middlewares de seguridad  
`npm install --save helmet`  

```js
var helmet = require('helmet');
app.use(helmet());
```

* **Input Validation**

    1. Chequear manualmente los datos con expresiones regulares de las rutas que
    aceptan datos externos  
    2. Deben tambien controlarse el mapeo de objetos de los datos   
    3. La validacion en el navegador es solo por criterios de usabilidad, no
    aporta proteccion a la aplicacion web   

`npm install --save express-validator`  

```js
var validator = require('express-validator');
// lo aplico despues del body-parser
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: true}));
app.use(validator());
```

Ahora en los manejadores tenemos acceso a `req.assert` y `req.validationErrors()`  

```js
app.post('/login', function(req, res){
  req.assert('password', 'Password is required').notEmpty();
  req.assert('email', 'A valid email is required').notEmpty().isEmail();
  var errors = req.validationErrors();
  if (errors) {
    res.render('index', {errors: errors});
  } else {
    res.render('login', {email: request.email});
  }
});
```

* **Seguridad de los paquetes npm**

```js
npm install -g nsp
nsp check --output summary
```

* **app.disable('x-powered-by');**

---

## TIPS

---

## DEPLOY

---

## HANDLEBARS

[HANDLEBARS](http://handlebarsjs.com/)

### instalacion

#### **servidor express**

`Renderizar` es el proceso de combinar datos con plantillas

* **`npm install --save hbs`**

```js
app.set('views', path);
app.set('view engine', name);
// path es la tuta a la carpeta con las plantillas
// name es el motor de plantilla a usar: ejs, jade, handlebars ...
app.engine() // para hacer cosas raras

app.set('views', path.join(__dirname, 'templates'));
app.set('view engine', 'hbs');
```

* **`npm install --save express-handlebars`**

Por defecto express busca las vistas en el subdirectorio `views/layouts`  

```js
var exphbs = require('express-handlebars');
app.engine('.hbs', exphbs({
  defaultLayout: 'default | main | loQueQueramos',
  extname: '.hbs',
  layoutsDir: path.join(__dirname, 'views/layouts'),
  partialsDir: path.join(__dirname, 'partials')
}));
app.set('view engine', '.hbs');
app.set('views', path.join(__dirname, 'views'));
// en las rutas tambien podemos configurar
app.get('/', function (request, response) {
  response.render('home', {
    name: 'John' ,
    layout: 'default | uOtro'
  });
});
```

* **consolidate**

`npm install --save consolidate`  
`npm install --save handlebars`  

```js
var consolidate = require('consolidate');
app.engine('html | hbs | handlebars', consolidate.handlebars);
app.set('view engine', 'html | hbs | handlebars');
app.set('views', path.resolve(__dirname , 'views'));
```

especificar el layout para cada plantilla o poner uno global

```js
res.render('view', { title: 'my other page', layout: 'otherLayout' });
app.set('view options', { layout: 'other' });
```

#### **cliente**

```js
<!DOCTYPE HTML>
<html>
<head>
<title>Handlebars Quickstart</title>
<!-- copia de handlebars o tiramos de cdn -->
<script type ="text/javascript" src="handlebars-v4.0.5.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com
                  /ajax/libs/handlebars.js/4.0.5/handlebars.js">
</script>
</head>
<body>
  <script>
    var src = "<h1>Hello {{name}}</h1>";
    var template = Handlebars.compile(src);
    var output = template({name: "Tom"});
    document.body.innerHTML += output;
  </script>
</body>
</html>
```

### comentarios

```html
{{! super-secret comment }}   // esto no saldra del servidor
<!-- not-so-secret comment -->
```

### expresiones

Una expresion es un `{{ contenido }}` que significa busca la propiedad contenido
dentro del contexto actual  
Si no queremos que se escapa ningun valor usamos `{{{ contenido }}}`


### helpers built-in

* **if**

```js
{{#if author}}
  <h1>{{firstName}} {{lastName}}</h1>
{{else}}
  <h1>Unknown Author</h1>
{{/if}}
```

* **unless** es lo contrario a if

```js
{{#unless license}}
  <h3 class="warning">WARNING: Not have a license!</h3>
{{/unless}}
```

* **each**

```json
<h1>Hello {{name}}</h1>
<ul>
  {{#each messages}}
    <li><b>{{from}}</b>: {{text}}</li>
  {{/each}}
</ul>
```

```js
app.get('/each', function (request, response) {
  var messages = [
    { from: 'John', text: 'Demo Message' },
    { from: 'Bob', text: 'Something Else' },
    { from: 'John', text: 'Second Post' }
  ];
  response.render('each', {
    name: 'Juan',
    messages: messages,
    layout: 'default'
  });
});
```

```js
// else que se mostrara solo cuando la lista este vacia
{{#each paragraphs}}
  <p>{{this}}</p>
{{else}}
  <p class="empty">No content</p>
{{/each}}

// {{@index}} es el numero de iteracion (indice)
{{#each array}}
  {{@index}}: {{this}}
{{/each}}

//  {{@key}} es el nombre de la clave (del par clave/valor)
{{#each object}}
  {{@key}}: {{this}}
{{/each}}
```

* **with** para cambiar el contexto del bloque  

* **lookup** para resolver valores de indices de arrays

### helpers custom

```js
var hbs = exphbs.create({
  // Specify helpers which are only registered on this instance.
  helpers: {
    foo: function () { return 'FOO!'; },
    bar: function () { return 'BAR!'; }
  }
});
app.get('/', function (req, res, next) {
  res.render('home', {
    showTitle: true,
    // Override `foo` helper only for this rendering.
    helpers: {
      foo: function () { return 'foo.'; }
    }
  });
});
```

### partials

Son para reusar componentes en diferentes paginas como por ejemplo un
componente que muestra el tiempo  

`{{> weather}}` - en la vista weather.handlebars  

```js
// middleware
app.use(function(req, res, next){
  if(!res.locals.partials) res.locals.partials = {};
  res.locals.partials.weather = getWeatherData();
  next();
});
```

---
