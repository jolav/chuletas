# EXPRESS 4.14

---

## BASICOS

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
app.use(express.static("public"));
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

[Handlebars con express.js](/backend/npm1/#handlebars)

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
app.use(logger("common | dev"));
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
`connect-cors` - soporte cors para express  
`connect-redis` - almacen de la sesion en redis  

---

## ROUTING

El ruteo determina como responde una aplicacion a una peticion de un cliente que
contiene una URI (o ruta) y un metodo HTTP (GET, POST ...)  
Cada ruta puede tener una o mas funciones manejadoras que se ejecutan cuando la
ruta coincide.  

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

---

## APIs

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

## TESTING

---

## TIPS

---

## DEPLOY

---









---
