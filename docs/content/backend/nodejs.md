# NODEJS 4

---

## NODEJS

### Arquitectura Asincrona

__Eventos sincronos__ con Bloqueo de I/O y multiples hilos

![nodejs](/z-static/images/nodejs/sincrono.png)

__Evento sincrono demultiplexado__ Sin bloqueo de I/O y un unico hilo. Las
tareas se reparten en el tiempo.

![nodejs](/z-static/images/nodejs/demultiplexado.png)

__Evento Asincrono__ la aplicacion expresa el interes de acceder a un recurso
en un momento dado ( sin bloquear I/O) y provee un manejador que sera invocado 
en otro momento cuando la operacion de acceso al recurso termine

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

4. Uncaught exceptions, se realizan con el try-catch visto en el ejemplo justo 
arriba

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

## API

![nodejs](/z-static/images/comingSoon2.jpg)

---
