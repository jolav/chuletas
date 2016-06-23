# EXPRESS 4

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
* Los middlewares son basicamente callbacks que se ejecutan cuando ocurre una
peticion HTTP
* Un middleware puede ejecutar cierta logica, devolver una respuesta o llamar
al siguiente middleware.
* La mayoria de los middleware son de diseño propio para cubrir nuestras
necesidades pero tambien los hay para cosas comunes como logueos, servir
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
* Para no terminar el ciclo un middleware debe pasar el control al siguiente
mediante un next();
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

Cada middleware registrado responde en principio a cualquier peticion
independientemente del path de la peticion  
Gracias a la caracteristica mounting de connect permite determinar que path se
requiere para que se ejecute el middleware

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

![express](/z-static/images/express/fileStructure.png)

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

![express](/z-static/images/express/traditional.png)
![express](/z-static/images/express/restApiAjax.png)

__Seguimiento de una peticion en express__

![express](/z-static/images/express/requestExpress.png)

### API de Express

* `application app` Es una instancia de la aplicacion express que se usa
 normalmente para configurar la aplicacion
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

![express](/z-static/images/express/expressChuleta.png)

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

![express](/z-static/images/express/codigosEstado.png)

### Seguridad

`Cross-site request forgery` Hay usar el middleware csrf  
`Permisos en procesos` no correr servicios como root y usar ubuntu authbin  
para puertos sin dar acceso root  
`HTTP Security Headess` instalar middleware helmet  
`Input Validation` instalar middleware express-validation  

---
