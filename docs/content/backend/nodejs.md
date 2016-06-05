# NODEJS 4

---

## NODEJS

### Arquitectura Asincrona

__Eventos sincronos__ con Bloqueo de I/O y multiples hilos 

![nodejs](/z-static/images/nodejs/sincrono.png)

__Evento sincrono demultiplexado__ Sin bloqueo de I/O y un unico hilo. Las tareas se reparten en el tiempo. 

![nodejs](/z-static/images/nodejs/demultiplexado.png)

__Evento Asincrono__ la aplicacion expresa el interes de acceder a un recurso en un momento dado ( sin bloquear I/O) y provee un manejador que sera invocado en otro momento cuando la operacion de acceso al recurso termine

![nodejs](/z-static/images/nodejs/reactorPattern.png)
 
### Closures

Con ellos podemos referenciar al entorno en el que una función se crea.  
Podemos mantener el contexto en el cual la operacion asincrona fue solicitada
sin importar cuando o donde su callback sera invocado

Ejemplo:

```javascript
(function() {
  var clickCount = 0;
  $('button#mybutton').click(function() {
    clickCount ++;
    alert('Clicked ' + clickCount + ' times.');
  });
}());

Al envolver todo con ()() evitamos contaminar el namespace global y 
hacemos que se invoque inmediatamente despues de definirse para crear
el nuevo scope
```

Ejemplo

```javascript
function parent() {
  var message = 'Hello World';
  function child() {
    alert (message);
  }
  return child;
}

var childFN = parent()
childFN()
```

### Callbacks

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

4. Uncaught exceptions, se realizan con el try-catch visto en el ejemplo justo arriba 

### Modulos

#### Patrones

__Patrones de diseño para creacion de modulos. Varios de los mas usados :__

1. __Patron exportar el nombre__ creando un namespace con el nombre del 
modulo, donde las funciones exportadas son propiedades del modulo cargado 

```javascript
//file logger.js
exports.info = function(message) {
  console.log('info: ' + message);
};
exports.verbose = function(message) {
  console.log('verbose: ' + message);
};
//file main.js
var logger = require('./logger');
logger.info('This is an informational message');
logger.verbose('This is a verbose message');

```

2. __Patron exportar una funcion__ exportamos una unica funcion, se puede
 ampliar usando la funcion exportada como namespace para otras APIs publicas
como se ve en el ejemplo 

```javascript
//file logger.js
module.exports = function(message) {
  console.log('info: ' + message);
};
module.exports.verbose = function(message) { //extiende el namespace
  console.log('verbose: ' + message);
};
//file main.js
var logger = require('./logger');
logger('This is an informational message');
logger.verbose('This is a verbose message');
```
3. __Patron exportar un constructor __

4. __Patron exportar una instancia __

5. __y mas...__

#### Cargar Modulos

* Modulo del core

```javascript
var http = require('http');
```

* Modulo de un fichero local

```javascript
var miModulo = require('./miModulo.js');
//ruta relativa o absoluta al archivo, mucho mejor relativa
```

* Modulo de una carpeta local

```javascript
var miModulo = require('./miModuloDir');
// buscara el package.json que lo define todo
```

* Modulo npm de la carpeta node_modules

```javascript
var miModulo = require('miModulo.js');
// busca directamente dentro de ./node_modules
// lo mejor usar npm y dejar que esto lo gestione
```



### Clase EventEmitter

__Observer Pattern (Patron del observador)__ define un objeto llamado sujeto
 que puede notificar a un conjunto de observadores (u oyentes) cuando ha 
ocurrido un cambio en su estado  
Esto se consigue en nodejs a traves de la clase `EventEmitter`

```javascript
var EventEmitter = require('events').EventEmitter;
var eeInstance = new EventEmitter();
```

__Metodos de EventEmitter__   
Los `listener` son funciones que se ejecutan al realizarse el evento 

```javascript
on(event, listener);
once(event, listener);
emit(event, [arg1], [...]);
removeListener(event, listener);
```


---

## CONNECT

### Connect

`Connect` es un modulo que envuelve la API de bajo nivel de node.js para 
facilitar los frameworks para aplicaciones web

* Servidor web con node.js basico

```javascript
var http = require('http');
http.createServer(function(req, res) {
  res.writeHead(200, {
    'Content-Type': 'text/plain'
  });
  res.end('Hello World');
}).listen(3000);
console.log('Server running at http://localhost:3000/');
```

* Servidor web con connect

```javascript
var connect = require('connect');
var app = connect();
app.listen(3000);
console.log('Server running at http://localhost:3000/');
```

### Middleware

* Son los componentes modulares que usa connect para gestionar las peticiones
HTTP hechas al servidor, manejarlas y responderlas
* Los middlewares son basicamente callbacks que se ejecutan cuando ocurre una peticion HTTP
* Un middleware puede ejecutar cierta logica, devolver una respuesta o llamar
al siguiente middleware.
* La mayoria de los middleware son de diseño propio para cubrir nuestras necesidades pero tambien los hay para cosas comunes como logueos, servir
ficheros estaticos, parseos y muchas mas cosas

__Cada funcion middleware tiene tres argumentos:__

1. `req`: objeto que contiene toda la informacion de la peticion HTTP
2. `res`: objeto que contiene toda la informacion de la respuesta HTTP
3. `next`: es el siguiente middleware definido en el orden de middlewares

__Registrar un middleware__ que ya tenemos definido en la aplicacion Connect:

```javascript
app.use();
```

Ejemplo de antes ahora devolviendo respuesta a http://localhost:3000 

```javascript
var connect = require('connect');
var app = connect();
var helloWorld = function(req, res, next) {
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
};
app.use(helloWorld);
app.listen(3000);
console.log('Server running at http://localhost:3000/');
```

__ Para cada ciclo request-response de una aplicacion Express__

* Se ejecutan los middlewares de arriba a abajo
* Para no terminar el ciclo un middleware debe pasar el control al siguiente mediante un next();
* next('route'); se salta el resto de middlewares del stack de rutas

#### Tipos

```javascript
- de aplicacion: atados a una instancia de express
    var app = express();
    app.use() ó app.METHOD() [METHOD = GET, PUT, POST, UPDATE ...]
- de ruta: atados a una instancia de express.Router()
    var router = express.Router();
    router.use() ó router.METHOD() [METHOD = GET, PUT, POST, UPDATE ...]
- de error: tienen un argumento mas (err, req, res, next)
- de builtin: los antiguos de connect
- de terceras-partes:
    npm install middleware;
    var middleware = require('middleware');
    app.use(middleware());
```

#### Esenciales

```sh
- compression             - express-static (o serve-static)
- morgan(antiguo logger)  - connect-timeout               
- body-parser             - errorhandler                - connect-busboy
- cookie-parser           - method-override             - serve-index
- express-session         - response-time               - passport
- csurf                   - serve-favicon               - vhost
```

### Mountain

Cada middleware registrado responde en principio a cualquier peticion independientemente del path de la peticion  
Gracias a la caracteristica mounting de connect permite determinar que path se requiere para que se ejecute el middleware

```javascript
var connect = require('connect');
var app = connect();

var logger = function(req, res, next) {
  console.log(req.method, req.url);
  next();
};
var helloWorld = function(req, res, next) {
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
};
var goodbyeWorld = function(req, res, next) {
  res.setHeader('Content-Type', 'text/plain');
  res.end('Goodbye World');
};

app.use(logger);
app.use('/hello', helloWorld);
app.use('/goodbye', goodbyeWorld);
app.listen(3000);

console.log('Server running at http://localhost:3000/');
```

---

## EXPRESS

### Instalacion

`npm install -g express`  
`npm install -g express-generator`

### Crear Proyecto

`express -h` Muestra las opciones  
`express --hbs miApp` Crea el proyecto miApp con handlebars pej  
`cd miApp & npm install` Instala las dependencias  
`DEBUG=miApp npm start` Arranca la aplicacion  

### Estructura archivos

![nodejs](/z-static/images/nodejs/fileStructure.png)

### Main file

__`app.js` o `main.js`__ Cosas que se hacen en este fichero:

1. Dependencias : Incluir paquetes de terceros y modulos nuestros como
    controladores, utilidades ayudantes y modelos
2. Instanciaciones : configurar las opciones de express como motor de plantillas
3. Configuraciones : como conectar las bases de datos
4. Middlewares : Definir middlewares
5. Ruteo : definir rutas
6. Arranque : iniciar la app o exportarla app como un modulo

__Funcionamiento cliente-servidor__

![nodejs](/z-static/images/nodejs/traditional.png)
![nodejs](/z-static/images/nodejs/restApiAjax.png)

__Seguimiento de una peticion en express__

![nodejs](/z-static/images/nodejs/requestExpress.png)

### API de Express

* `application app` Es una instancia de la aplicacion express que se usa normalmente para configurar la aplicacion
* `request req` Es una envoltura del objeto request del modulo HTTP usado 
para extraer informacion sobre la peticion
* `response res` Es una envoltura del objeto response del modulo HTTP usado
 para enviar los datos y cabeceras de la respuesta

__Application settings:__

* env: production, development ...
* viewcache
* view engine: jade, ejs, hbs ...
* views
* trustproxy (NGINX)

![nodejs](/z-static/images/nodejs/expressChuleta.png)

### Motor de plantillas

`Renderizar` es el proceso de combinar datos con plantillas

```javascript
- Para decirle a express cual vamos a usar
app.set('views', path);
app.set('viewengine', name);
// path es la tuta a la carpeta con las plantillas
// name es el motor de plantilla a usar: ejs, jade, handlebars ...
app.engine() es un metodo de bajo nivel para decirle a express que 
    motor de plantillas vamos a usar
```
### Middleware

[Middleware en Connect](#middleware)  
  
### Routing

```sh
protocolo  hostname       port    path       querystring     fragment

http://    localhost      :3000   /about     ?test=1         #history
http://    www.bing.com           /search    ?q=grunt&first
https://   google.com             /                          #q=express
```

#### app.METHOD

`app.METHOD`(path, [callback], callback);

```javascript
- path es la ruta en el servidor que pueden ser cadenas, patrones de 
  cadenas o expresiones regulares
- callback es la funcion que se ejecuta cuando la ruta coincide.
- Cuando hay varios callbacks para evitar que se ejecuten todos hay que 
  invocar next('route') y saltara a la siguiente
METHOD :    - get, leer
            - post, crear
            - put, actualizar
            - delete, borrar
            - hay otros cuantos
            - all, para todos
```

#### Router class

```javascript
var express = require('express');
var profile = express.Router();

// middleware especifico para esta ruta si nos hace falta, es opcional
profile.use (function(req, res, next) {
  ...
  next();
});
profile.get("/", function(req, res, next) {
  ...
});
profile.get("/username", function(req, res, next) {
  ...
});
module.exports = profile;
```

```javascript
// en app.js con esto creamos dos rutas "/profile" y "/profile/:username"
app.use('/profile', require('./rotes/profile'));

//Cada router puede cargar otros router hasta cargar todos en pej index.js
// y al cargar en app,js /controller/index.js ya estaran todas las rutas
```

#### app.route()

cadena de manejadores de ruta

```javascript
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

### Manejo de errores

* En desarrollo 

```javascript
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

![nodejs](/z-static/images/nodejs/codigosEstado.png)

### Seguridad

`Cross-site request forgery` Hay usar el middleware csrf  
`Permisos en procesos` no correr servicios como root y usar ubuntu authbin  
para puertos sin dar acceso root  
`HTTP Security Headess` instalar middleware helmet  
`Input Validation` instalar middleware express-validation  

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

`Semver` versiones semanticas
![nodejs](/z-static/images/nodejs/semver.png)

---

## REST

![nodejs](/z-static/images/comingSoon2.jpg)

---

## SOCKET.IO

![nodejs](/z-static/images/nodejs/httpVerbs.jpg)

---

## FEATHERS.JS

![nodejs](/z-static/images/comingSoon2.jpg)

---

## ASYNC

[GitHub Async](https://github.com/caolan/async)  
[npm Async](https://www.npmjs.com/package/async)  

![nodejs](/z-static/images/nodejs/callback1.jpg)

---

## PASSPORT

[Pagina Web Passport](http://passportjs.org)  
[npm Passport](https://www.npmjs.com/package/passport)  
 
---







