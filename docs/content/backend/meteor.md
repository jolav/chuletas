# METEOR 1.2

---

## METEOR

### Instalacion

* Instalar `curl https://install.meteor.com/ | sh`
* Desinstalar :  Especialmente cuando sale el error de no reconocer modulos el 
primero de los cuales es fibers.  

` rm -rf ~/.meteor`  
` rm /usr/bin/meteor`

### Scope - Visibilidad

`var variable` Variable es local a la funcion donde se declara o si esta fuera de una funcion es local al archivo donde se declara  
`variable` variable es global a toda la aplicacion meteor  

`var name = function(){};` local al archivo  
`function name(){}` local al archivo  
`name = function(){}` global a toda la aplicacion meteor   

### Estructura

`/client` para plantillas, eventos, ayudantes y subscripciones  
`/server` para metodos y publicaciones  
`/both` para colecciones  
`/public` para visitantes, se sirve como contenido estatico tambien. p ej: imagenes, favicon, robots.txt o contenido estatico  
`/lib` se cargan antes que todo lo demas  
`/tests` meteor ni los carga ni los toca para nada  
`/packages` para usar paquetes locales  
`main.*` estos ficheros se cargan los ultimos  

`Meteor.startup(function) {}`

```javascript
Meteor.startup(function)
// en el servidor se ejecuta despues de que todo lo demas del servidor se
// ha ejecutado por lo que permite el acceso a variables globales
// en el cliente se ejecuta en cuanto el DOM esta listo

Meter aqui todo el codigo que no este en eventos, helpers, metodos, 
publicacioneso subscripciones para asegurarse que no se ejecuta nada 
antes de que el entorno este preparado
```

`/private` solo accesible a codigo ejecutado en el servidor. Nunca accesible
a los usuarios

```javascript
// Para cargar ficheros de la zona privada usamos la API Assets
// Sincrono
var myData = Assets.getText('data.txt');
// Asincrono
Assets.getText('data.txt', function(error, result){
    // Hacer lo que sea con el result
    // Si hay error hacer lo que sea con error
});
```

![meteor1](/z-static/images/meteor/folderStructure.png)

---

## TEMPLATES

Se inserta en el archivo .html `{{> nombrePlantilla}}` 

```handlebars
<template name="nombrePlantilla">
  lo que sea
</template>
```

### Refactorizar Plantillas

Cuando cambia un dato de una coleccion las plantillas donde se muestra ese dato cambian inmediatamente.  
Cuanto mas pequeñas sean las plantillas cuando un dato cambie, solo las plantilas mas pequeñas tendran que cambiar  
 
```handlebars
<template name="todos">
  {{#each todo}}
    {{name}}
  {{/each}}
</template>

 pasa a ...

<template name="todos">
  {{#each todo}}
    {{> todoItem}}
  {{/each}}
</template>

<template name="todoItem">
  {{ name}}
</template>
```

### Contexto de datos

```handlebars
<template name="contextExample">
  <p>{{ someText}}</p>
</template>
// En otra plantilla insertamos
{{>contextExample someText="Creado como argumento en plantilla padre"}
```

### Contexto Dinamico

```handlebars
<template name="contextExample">
  <p>{{ someText}}</p>
  <p>{{someNested.text}}</p>
</template>

// Este helper en la plantilla a insertar plantillaX
'dataContextHelper': function () {
  return {
    someText: 'Texto creado usando un helper en la plantilla padre',
    someNested : {text:'Esto viene de someNested.text'}
  };
}

// Insertamos en la plantillaX
<template name=plantillaX">
  {{> contextExample dataContextHelper}}
</template>
```

`with`

```handlebars
{{#with	dataContextHelper}}
  {{>	contextExample}}
{{/with}}

{{#with dataContextHelper}}
  <p>{{someText}}</p>
  <p>{{someNested.text}}</p>
{{/with}}
```

### Instancias de plantilla

* Usar para acceder a elementos HTML de la plantilla  
* Se le pueden asignar propiedades que persisten mientras la plantilla se actualiza reactivamente  
* Las propiedades que añades a la instancia de la plantilla las puedes usar en helpers y manejadores de eventos  
* Usa los callbacks onCreated y onDestroyed para inicializarlas y limpiarlas

Los objetos instanciados de plantillas existen en : 
 
1. this en callbacks de plantilla onCreated, onRendered y onDestroyed  
2. el segundo argumento de los manejadores de eventos  
3. Template.instance() en los helpers  
 
---

## HELPERS

Las funciones de los helpers son reactivas 

### Globales

```javascript
Template.registerHelpers('nombreHelper', function(param1, param2, ...) {
  codigo funcion;
});
Se puede usar en cualquier plantilla como {{ registerHelper ...}}
o en cualquier sitio en el cliente como
var result = UI._globalHelpers.nombreHelper(param, param2, ...) ?????
```

### Locales

Los helpers se insertan en el template `{{ nombreHelper}}`  
En estos helpers `this` se refiere al contexto de datos actual  

```javascript
Template.nombrePlantilla.helpers() {
  'nombreHelper': function() {
    codigo del helper;
  },
  'nombreHelper2': function() {
    codigo del helper;
  }
}
```

Ademas de los helpers de diseño propio tenemos 3 callbacks que se llaman 
cuando la plantilla de crea, se renderiza y se destruye  
`onCreated` , `onRendered` , `onDestroyed`  
`created`, `rendered` y `destroyed` estan deprecados.  
En estos callbacks `this` se refiere a la plantilla   

```javascript
Template.nombrePlantilla.onCreated(function() {
  Justo antes de que la logica de la plantilla se evalue por primera vez
  Es el primero de los tres;
  bueno para establecer valores que luego leeran los Template.helpers;
});
Template.nombrePlantilla.onRendered(function() {
  Cuando la plantilla se inserta en el DOM
  bueno para hacer inicializaciones y hacer cosas con el DOM;
});
Template.nombrePlantilla.onDestroyed(function() {
  Cuando la plantilla es eliminada del DOM
  para limpiar
});
```

`Template.instance()`  
Para acceder a la instancia plantillas desde dentro de los helpers de plantilla 

### Pasar datos a helpers

__Como parametros__  
`{{ miHelper "A String" aContextProperty}}`

```javascript
Template.nombrePlantilla.helpers({
  'miHelper': function (myString, myObject) {
    myString = "A String";
    myObject = aContextProperty;
  }
});
```

__Como pares valor-clave__  
`{{ miHelper myString="A String" myObject=aDataProperty}}`

```javascript
Template.nomnbrePlantilla.helpers({
  'miHelper': function(params){
    params.hash.myString = 'A String';
    params.hash.myObject = aDataProperty;
  }
});
```

__Inclusiones helpers__ son diferentes, esperan pares clave-valor u objetos  
`{{> miPlantilla unaCadena="Aparecere dentro de la plantilla}}`  
`{{> miPlantilla objetoConDatos}}` 
Para pasar una simple variable a una inclusion o a un block helper Meteor lo convierte en un objeto   

```javascript
{{#myBlock "someString"}}
  ...
{{/myBlock}}
Si lo queremos usar en un block helper hay que castear el argumento
Template.myBlock.helpers({
  'hazAlgoConCadena': function(){
  String(this); //castear la cadena para usarla
  return this
  }
});
<template name="myBlock">
  <h1>{{ this}}</h1>
  {{ Template.contentBlock}}
</template>
```

---

## EVENTOS

Los eventos se asocian a templates  
Al ocurrir un evento `this` se refiere al objeto en el que salta el evento y 
ese objeto tiene todas propiedades `this.name` `this._id` `this.loquesea`  
Los manejadores de eventos tienen dos argumentos: el objecto evento y la 
instancia de la plantilla   

```javascript
Template.nombrePlantilla.events() {
  'click .clase': function(event, template) {
    event.preventDefault() // p.ej en form para no recargar la pagina;
    codigo;
  },
  'keyup [name=nombreQueSea]: function(event, template) {
    codigo;
  }
}
```

Nombre de las funciones de eventos

1. Nombre del evento : `click`, `dblclick`, `focus`, `blur`, `mouseover`, 
`change`, `mousenter`, `mouseleave`, `mousedown`, `mouseup`, `keydown`, 
`keypress`, `keyup`, `submit form`, `change [type=checkbox]`
2. Selector CSS (tambien en Jquery) que indica el elemento a escuchar

---

## SPACEBARS

[Manual de Spacebars](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md)

### Template Tags

* Expresiones: valor de un helper o propiedad del objeto para insertar cadenas
 de texto 

```javascript
{{ name}}
{{ miObjeto.propiedad}}
{{ ../propiedadDePlantillasPadres}}
{{ ../../algunaPropiedadPadres}}
Para hacer lo mismo dentro de un helper usamos API Template.parentData(n)
n - pasos arriba para llegar al contexto de datos de las plantillas padres
Template.parentData(0) = Template.currentData() = this en un helper
```

* Inclusiones para insertar otras plantillas por el nombre 

```javascript
{{> templateName}}
```

* Para insertar HTML crudo 
```javascript
{{{ contenidoConHtml}}}
```

### Block Helpers

* if__ else

```javascript
{{#if something}}
  It's true
{{else}}
  It's false
{{/if}}
```

* unless

```javascript
unless es lo mismo que #if pero con la condicion al reves
```

* with

```javascript
establece un nuevo contexto de datos a su contenido y plantillas 
contenidas
```

* each

```javascript
// El argumento suele ser como aqui un cursor, pero tambien puede ser
//un array de javascript, null o undefined
return coleccion.find();
{{#each player}}
  {{ name}} : {{score}}
{{/else}}
  esto se ejecuta si player esta vacio
{{/each}}
```

### Block Helpers diseño propio


```javascript
<template name="blockHelperExample">
  <div>
    <h1>Mi block helper de diseño</h1>
    {{#if this}}
      <p>Contenido va aqui: {{> Template.contentBlock}}</p>
    {{else}}
      <p>Sino contenido va aqui: {{> Template.elseBlock}}</p>
    {{/if}}
  </div>
</template>

// Añadiriamos la plantilla que quisieramos
{{#blockHelperExample true}}
  Contenido para verdadero
{{else}}
  Contenido para falso
{{/blockHelperExample}}
```

Tambien podemos usar un helper para cambiar el valor y asi obtener un block
helper dinamico  
Ademas podemos pasar argumentos clave-valor y acceder a ellos por su clave
dentro de la plantilla del block helper como por ej   

```javascript
{{#blockHelperExample myValue=true}}
  ...
{{/blockHelperExample}}
```

O acceder por su nombre dentro de plantilla como por ejemplo    

```javascript
<template name="blockHelperExample">
  <div>
    <h1>Mi block helper de diseño</h1>
    {{#if myValue}}
      ...
    {{/if}}
  </div>
</template>
```

Con todo esto se ve que se pueden crear componentes listos para usar que se 4 empaqueten en paquetes para su distribucion y uso de terceros   

---

## METODOS

### Ej teorico

Siempre borra el paquete :  
`meteor remove insecure` para evitar CRUD desde consola  
Los metodos son codigo que se ejecuta en el servidor despues de haber sido disparado en el cliente  
Compensacion de la latencia Para evitar el retraso de que al modificar algo vaya al servidor, se ejecute el metodo, se guarden los cambios en la base de datos, vuelvan al cliente y se muestren en la interfaz  
Para evitar eso los metodos se pueden ejecutar en cliente y en servidor tal que en el cliente minimongo realiza los cambios instantaneamente y se reflejan en la interfaz  
`check(variable, tipoDeObjeto)`  
Si casa todo sigue igual  
Si no casa salta un error y el resto del metodo no se ejecuta  
`throw new Meteor.error(tipo de error, mensaje de error)`  

Usamos el patron de data para guardar los datos  
No confies nunca en los ID que vienen de clientes  

```javascript
// En el cliente
Meteor.call ('metodo1', param1, param2, ..., paramN, callback);

// En el servidor
Meteor.methods({
  'metodo1': function(parametros) {
    codigo;
  },
  'metodo2': function(parametros) {
    codigo;
  }
});
```

### Ej basico sin callback

```javascript
Meteor.call('updateListItem', documentId, todoItem);

Meteor.methods({
  'updateListItem': function (documentId, todoItem) {
    check(todoItem, String);
    var currentUser = Meteor.userId();
    var data = {
      _id: documentId,
      createdBy: currentUser
    }
    if(!currentUser){
      throw new Meteor.Error("not-logged-in", "You're not logged-in.");
    }
    Todos.update(data, {$set: { name: todoItem }});
  }
});
```

### Ej basico con callback

```javascript
Template.addTodo.events({
  'submit form': function (event) {
    event.preventDefault();
    var todoName = event.target.todoName.value;
    var currentList = this._id;
  Meteor.call('createListItem', todoName, currentList, function(error){
      if(error){
        console.log(error.reason);
      } else {
        event.target.todoName.value = "";
      }
    });
  }
});

Meteor.methods({
  'createListItem': function (todoName, currentList) {
    check(todoName, String);
    check(currentList, String);
    var currentUser = Meteor.userId();
    var data = {
      name: todoName,
      completed: false,
      createdAt: new Date(),
      createdBy: currentUser,
      listId: currentList
    }
    if (!currentUser) {
      throw new Meteor.Error("not-logged-in", "You're not logged-in.");
    }
    var currentList = Lists.findOne(currentList);
    if(currentList.createdBy != currentUser){
      throw new Meteor.Error("invalid-user", "You don't own that list.");
    }
    return Todos.insert(data);
  }
});
```

---

## PUBLICACIONES

Siempre borra el paquete :  
`meteor remove autopublish` para controlar las publicaciones usando en su lugar 

```javascript
// En el servidor
Meteor.publish('listaNombres', function(){
  return coleccionQueSea.find();
});
// En las funciones de publicacion usar this.userId y no Meteor.userId()
```

### Patrones

#### Globales

```javascript
Todas por ejemplo en /clients/subscripciones.js
Meteor.subscribe("posts");
```

#### En el router

__`iron:router`__

```javascript
Router.route('/post/:_id', {
  subscriptions: function() {
    // returning a subscription handle or an array of subscription handles
    // adds them to the wait list.
    return Meteor.subscribe('item', this.params._id);
  },
  action: function () {
    if (this.ready()) {
      this.render();
    } else {
      this.render('Loading');
    }
  }
});
```

__`flowrouter__

```javascript
FlowRouter.route('/blog/:postId', {
  subscriptions: function(params) {
    this.register('myPost', Meteor.subscribe('blogPost', params.postId));
  }
});
```

#### En las plantillas

[Joins en plantillas](http://dweldon.silvrback.com/template-joins)    

[Subscripciones en plantillas](https://www.discovermeteor.com/blog/template-level-subscriptions/)


```javascript
Template.posts.onCreated(function () {
  // 1. Initialization
  var instance = this;
  // initialize the reactive variables
  instance.loaded = new ReactiveVar(0);
  instance.limit = new ReactiveVar(5);
  // 2. Autorun
  // will re-run when the "limit" reactive variables changes
  instance.autorun(function () {
    // get the limit
    var limit = instance.limit.get();
    console.log("Asking for "+limit+" posts…")
    // subscribe to the posts publication
    var subscription = instance.subscribe('posts', limit);
    // if subscription is ready, set limit to newLimit
    if (subscription.ready()) {
      console.log("> Received "+limit+" posts. \n\n")
      instance.loaded.set(limit);
    } else {
      console.log("> Subscription is not ready yet. \n\n");
    }
  });
  // 3. Cursor
  instance.posts = function() {
    return Posts.find({}, {limit: instance.loaded.get()});
  }
});
```

```javascript
// lists.js
Template.lists.onCreated(function () {
  this.subscribe('lists');
})
// lists.html
{{#if Template.subscriptionsReady}}
  {{#each list}}
    <li><a href="{{pathFor route='listPage'}}">{{name}}</a></li>
  {{/each}}
{{else}}
  <li>Loading...</li>
{{/if}}
// router.js
Router.route('/list/:_id', {
  name: 'listPage',
  template: 'listPage',
  waitOn: function(){
    var currentList = this.params._id;
    return Meteor.subscribe('todos', currentList);
  }
});
```

### Parametros

```javascript
Meteor.publish('todos', function(currentList){
  var currentUser = this.userId;
  return Todos.find({ createdBy: currentUser, listId: currentList })
});
```

```javascript
subscriptions: function () {
  var currentList = this.params._id;
  return Meteor.subscribe('todos', currentList);
}
```

---

## MONGODB

### Crear Usuarios

```javascript
$ mongo
> use admin
> db.createUser({user:"usuarioAdmin",pwd:"contraseña",
>                   roles:[{role:"root",db:"admin"}]})

$ mongo -u usuarioAdmin -p contraseña --authenticationDatabase admin
> use some_db
> db.createUser({user: "usuario",pwd: "contraseña",roles: ["readWrite"]})

mongod --repair //cuando se interrumpe el servicio de mongo
mongod --auth //para iniciar el servicio con la autenticacion activada
```

### Crear colecciones

```javascript
miTabla = new Mongo.Collection('mitabla');
miTabla = new Mongo.Collection(null); // crea coleccion solo en cliente
Meteor.Collection esta deprecado
```

`IdGenerator`

MongoDB por defecto usa ObjectId, por ejemplo al usar mongoimport.  
Meteor por defecto usa string  

```javascript
// crea "_id" : "6Ar8tad67Tru9TH"
('mitabla', {idGenerator: 'MONGO'}); 
// crea "_id" : ObjectId ("9Uyt06iG39gh9")
('mitabla', {idGenerator: 'STRING'});
```

### Minimongo

Minimongo es el encargado de la base de datos en el lado cliente

`coleccion.find`

```javascript
coleccion.find();        // Nos devuelve todos los documentos en un cursor
coleccion.find().fetch() // Nos devuelve todos los documentos en un array 
                         // de objetos donde cada objeto es un documento
coleccion.find({documentosABuscar},{opcionesDeBúsqueda})
coleccion.findOne()         // para buscar un solo documento
Ordenar por 1 es orden ascendente, y -1 descendente
```

`coleccion.remove`

```javascript
coleccion.remove(_id)           // borra el documento con ese _id
```

`coleccion.insert`

```javascript
coleccion.insert(documento, callback);
```

`coleccion.update`

```javascript
coleccion.update(docAModificar, comoLoModificamos,multi/upsert, callback);
update por defecto borra todo el doc salvo la primary key y lo sustituye
por los cambios especificados
Para cambiar campos sin borrar el documento primero operador $set
```

### Replica set

![meteor1](/z-static/images/comingSoon2.jpg)

### Oplog Tailing

![meteor1](/z-static/images/comingSoon2.jpg)

---

## SESSION

La sesion es un almacen de datos global y reactivo. Cada usuario tiene su 
propio objeto Session 

```javascript
Session.set('nombreVariable', 'valor');
Session.get('nombreVariable);
```

`Session.equals('clave', valor)` dentro de un autorun anula la reactividad la proxima vez que la clave cambie a o desde el valor. Cuando se pueda es mas eficiente usar Session.equals() que Session.get()

`paquete reactive-var` Pueden ser creadas y usadas de forma local p ej 
asignadas a una instancia de una plantilla 

```javascript
var name = new ReactiveVar('valorName');
name.get();
name.set('valor');
// ejemplo en plantilla
Template.colorDelBoton.onCreated(function() {
  this.colorActual = new Reactivevar;
  this.color.set('azul');
}
```

`paquete reactive-dict` Sin var es global, con var es local al archivo o 
funcion donde se declara 

```javascript
sesRef = new ReactiveDict('sesionReferencias'); //declaracion global
sesRef.set('filasPorPagina', 10);
sesRef.set('tipoOrden', 'defaultSort');
sesRef.get('tipoOrden');
sesRef.get('filasPorPagina')
```

---

## REACTIVIDAD

### Metodo imperativo

Metodo imperativo `.observe()` funcion que dispara callbacks cuando los documentos seleccionados cambian. Asi podemos cambiar el DOM con esos
 callbacks.   
Util para widges de terceros como añadir o quitar puntos de una mapa en tiempo real que sean p ej usuarios conectados, los callbacks added y removed para 
llamar a los métodos dropPin() o removePin() de la API del mapa.  

```javascript
Posts.find().observe({
  // when 'added' callback fires, add HTML element
  added: function(post) { 
    $('ul').append('<li id="' + post._id + '">' + post.title + '</li>');
  },
  // when 'changed' cb fires,modify HTML element's text
  changed: function(post) { 
    $('ul li#' + post._id).text(post.title);
  },
  // when 'removed' cb fires,remove HTML element
  removed: function(post) { 
    $('ul li#' + post._id).remove();
  }
});
```

### Metodo declarativo

Es el que usamos. Se define la relacion entre los objetos una sola vez y se mantendra sincronizado  

### Fuentes de datos reactivas

```javascript
- Sesiones
- reactive-dict y reactive-var
- Minimongo
- Meteor.user(), Meteor.status()
```

### Computaciones

No todo el codigo en Meteor es reactivo, la reactividad se limita a areas delimitadas que llamamos computaciones  
`Computación` es un bloque de código que se ejecuta cada vez que cambia una de las fuentes de datos reactivos de las que depende

### Tracker

* `Tracker.autorun(function(){});` convierte en reactivos los datos del interior de la funcion. Cada vez que los datos cambian la funcion se ejecuta  
No es buena practica crear funciones reactivas globales, es mejor atarlas a plantillas, lo cual se hace en los callbacks onCreated() u onRendered(), ademas estas funciones reactivas se destruyen solas cuando la plantilla se destruye

```javascript
Template.nombrePlantilla.onCreated = function() {
  this.autorun( function() {
    alert(Session.get('ejemplo'));
  });
};
```

```javascript
Tracker.autorun(function () {
  var celsius = Session.get("celsius");
  Session.set("fahrenheit", celsius * 9/5 + 32);
});
```

*  `.stop()` para detener la reactividad de una funcion 
 
```javascript
Tracker.autorun(function (c) {
  if (comparacion con variables de sesion o lo que sea) {
    ......
  } else {
    c.stop(); // deja de ser reactiva
});

var funcionReactiva = Tracker.autorun(function() {
  ...
});
funcionReactiva.stop() // y deja de ser reactiva
```

* `Tracker.flush()` fuerza a completarse todas las actualizaciones reactivas pendientes  
Por ej si un evento cambia una variable de session que hara rerenderizarse la interfaz de usuario, si usamos flush forzamos la rerenderizacion inmediatamente y asi accedemos al nuevo DOM

---

## ERRORES

Creamos una coleccion local solo en el cliente  
`Errores = new Mongo.ColLection(null);`
  
Funcion para añadir errores     

```javascript
lanzarError = function(mensaje) {
  Errores.insert({mensaje: mensaje }):
}
```

Creamos una plantilla `errores.html` y añadimos en la layout donde queramos 
`{{> errores}}`  

Integramos el ayudante de plantilla   

```javascript
Template.errors.helpers({
  'errores': function() {
    return Errors.find();
)};
```

Cuando haya errores los mandamos a `lanzarError('errorQueSea')`  
Crear paquete para recoger los errores  

---

## PATRONES DISEÑO

Para pasar datos entre plantillas otra posibilidad es crear colecciones solo de cliente `tabla = new Mongo.Collection(null)`

---

## SEGURIDAD

### Metodos

`meteor remove autopublish`

### Publicacion/Subscripcion

`meteor remove insecure`  
[seguridad 1](http://0rocketscience.blogspot.com.es/2015/07/meteor-security-no-1-meteorcall.html)  
[seguridad 2](http://justmeteor.com/blog/meteor-security-fundamentals/)  

---

## TESTS

`Jasmine`  
`tynitest`  

---

## DEPLOY

### Hostings 

```sh
DO (digitalocean)
Linode
Vultr
Modulus // mas caro pero a un solo comando
```

### Metodos

```sh
Demeteorizer
Meteor Up (mup)
```

### Meteor Build

`aptitude install build-essential`  
`meteor build .` dentro de la carpeta del proyecto y nos generara un *.tar.gz  
Subimos al servidor el `*.tar.gz`

```sh
tar -zvxf *.tar.gz
cd bundle/programs/server
npm install
nano /home/usuario/.bash_profile
    export ROOT_URL=http://aplicateca.brusbilis.com //url o IP
    export MONGO_URL=mongodb://user:contraseña@localhost:27017/nameBBDD
    export PORT=3000 //por defecto usa el 80 pero solo se puede como root
source /home/usuario/.bash_profile // para recargar la configuracion
```

Aqui ya solo queda configurar nginx añadiendo la escucha para meteor/node a las otras que tengamos dentro del virtualhost  
`nano /etc/nginx/sites-available/brusbilis` 

```sh
server {
    listen 80 ;
    listen [::]:80 ;

    server_name     aplicateca.brusbilis.com;

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://127.0.0.1:3000/;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

`cp /etc/nginx/sites-available/brusbilis /etc/nginx/sites-enabled/brusbilis`   
`service nginx restart`  
Ahora ya `node main.js` desde el subdirectorio bundle  

Paquete de npm pm2 // ejecutar todo lo siguiente como root 

```sh
npm install pm2 -g
pm2 start main.js //pm2 levantara automaticamente main.js si se cae
pm2 startup debian //pm2 se ejecuta al inicio y levantara main.js
pm2 save
```

Otros comandos de pm2

```sh
pm2 kill
npm remove pm2 -g
pm2 list
upstart Es una alternativa a pm2
```

Falta ver como ejecutar mas una aplicacion node a la vez por servidor

---

## PAQUETES

### Importantes

`less` compila ficheros de estilo al vuelo a CSS  
`markdown` para poder usar el block helper {{# markdown}}  
`twbs:bootstrap bootstrap 3`  
`ian:accounts-ui-bootstrap-3`  
`rajit:bootstrap3-datepicker` elige fechas  
`sacha:spin` efecto de carga animado {{> spinner}}  
`multiply:iron-router-progress` No me gusta, barra arriba de carga  

`jeeeyul:moment-with-langs` parsea fechas y las formatea  
`wizonesolutions:underscore_string` genera slugs a partir de cadenas  
`themeteorchef:jquery-validation` Validacion formularios en cliente  
`matb33:collections-hooks` para ejecutar algo antes de cada create, update, delete, find o finddOne incluso en el lado del servidor  
`aldeed:autoform` formularios con update, insert y bootstrap3  
`aldeed:simple-schema` crear un esquema para las colecciones  
`alethes:pages` paginacion  

`msavin:mongol` ctrl+M en navegador para gestionar la bbdd  

`underscore` libreria para manipular estructuras de datos  
`reactive-dict`  
`reactive-var`  
`iron-router`  
`accounts-password`  
`accounts-ui`  
`reywood:publish-compositeJoins` reactivos  

### Paquetes Propios

![meteor1](/z-static/images/comingSoon2.jpg)

### Modulos NPM

![meteor1](/z-static/images/comingSoon2.jpg)

---

## IRON-ROUTER

[Guia Oficial](https://iron-meteor.github.io/iron-router/)

`Routes` instrucciones que dicen donde ir y que hacer al encontrarse con una 
URL  
`Path` direccion URL dentro de la aplicacion, estatica, dinamica o con
 parametros de consulta  
`Segmentos` las partes del path  
`Hooks` acciones a realizar antes, durante o despues del enrutamiento p ej: comprobar credenciales antes de de mostrar la pagina  
`Filters` hooks globales para una o mas rutas  
`Layout` marcos de contenido en HTML  
`Route template` cada ruta debe apuntar a una plantilla  
`Controllers` para no duplicar codigo en situaciones parecidas  

`{{> yield}}` Es el Helper donde se renderizan las plantillas de la ruta  

`var currentRoute = Router.current().route.getName()` Nos da la ruta donde estamos  
`Router.map()` DEPRECADO  

### Hooks

```sh
action // Puede sobreescribir el comportamiento por defecto de la ruta
          Si la definimos tenemos que renderizar manualmente la plantilla
          usando this.render()
subscriptions
waitOn // devuelve suscripciones y mientras carga la plantilla de carga
data // mas abajo
```

```javascript
Router.onRun // Cuando la ruta se ejecuta por primera vez
Router.onRerun // similar a onBeforeAction, hau que usar this.next()
Router.onBeforeAction // Antes de ejecutar la ruta, usar this.next()
Router.onAfterAction // Se ejecuta despues de haber ejecutado la ruta
Router.onStop // Cuando la ruta se para, antes de una nueva ruta
```

`onBeforeAction` para asegurarnos que antes de acceder a una pagina esta 
logueado 

```javascript
onBeforeAction: function(){
  var currentUser = Meteor.userId();
  if(currentUser){
    this.next();
  } else {
    this.render("login");
  }
}
```

### Data

Pasamos datos a una ruta desde la funcion asociada a data. Aqui pasamos a la plantilla listPage a la que vamos los datos del return de la funcion data   

```javascript
Router.route('/list/:_id', {
  name: 'listPage',
  template: 'listPage',
  data: function () {
    var currentList = this.params._id;
    var currentUser = Meteor.userId();
    return Lists.findOne({_id: currentList, createdBy: currentUser});
  }
});
```

### Plantillas

`Plantillas por defecto` para todas las rutas las configuramos como una opcion global de ruta  

```javascript
Router.configure({
  layoutTemplate: 'plantillaEstandar',
  notFoundTemplate: 'plantillaParaRutaNoEncontrada',
  loadingTemplate: 'plantillaDeCarga'
});
```

`Plantillas personalizadas`  
 
```javascript
<template name="ApplicationLayout">
  <header>
    <h1>{{title}}</h1>
  </header>
  <aside>
    {{> yield "aside"}}
  </aside>
  <article>
    {{> yield}}
  </article>
  <footer>
    {{> yield "footer"}}
  </footer>
</template>

```

Le decimos al router que plantilla usaremos 

```javascript
Router.route('/post/:_id', function () {
  this.layout('ApplicationLayout');
});
```

Tenemos las plantillas   

```javascript
<template name="Post">
  <p>{{post_content}}</p>
</template>
<template name="PostFooter">
  Some post specific footer content.
</template>
<template name="PostAside">
  Some post specific aside content.
</template>
```

Tambien podemos indicarlo asi 

```javascript
<template name="Post">
  <p>{{post_content}}</p>
  {{#contentFor "aside"}}
    Some post specific aside content.
  {{/contentFor}}
  {{#contentFor "footer"}}
    Some post specific footer content.
  {{/contentFor}}
</template>
```

Y le decimos el que en la ruta 

```javascript
Router.route('/post/:_id', function () {
  // use the template named ApplicationLayout for our layout
  this.layout('ApplicationLayout');
  // render the Post template into the "main" region
  // {{> yield}}
  this.render('Post');
  // render the PostAside template into the yield region named "aside"
  // {{> yield "aside"}}
  this.render('PostAside', {to: 'aside'});
  // render the PostFooter template into the yield region named "footer"
  // {{> yield "footer"}}
  this.render('PostFooter', {to: 'footer'});
});
```

### Redirecciones

`Router.go('urL');`  
Aqui la redireccion esta en un callback que se ejecutara cuando la lista sea correctamente insertada en la coleccion.   

```javascript
'submit form': function (event) {
  event.preventDefault();
  var listName = event.target.listName.value;
  Lists.insert({
    name: listName
  }, function (error, results) {
    Router.go('listPage', {_id: results});
  });
  event.target.listName.value = "";
}
```

Redireccionar todas las rutas no existentes a una.  

```javascript
Router.route('/(.*)', {
  action: function() {
    Router.go('index');
  }
});
```

### Ejemplo Basico

Todo el codigo en html que no pase por router aparecera en cada pagina de la 
app  
 
```html
<head>
  <meta charset="UTF-8">
  <title>TITULO</title>
</head>
Aparecera el TITULO en todas las paginas
```

```javascript
Router.configure({
  layoutTemplate: 'main'
});
Router.route('/', {
  template: 'home'
});
Router.route('/register', {
  template: 'register'
});
Router.route('/login', {
  template: 'login'
});

<template name="main">
  <h1>APLICACION TODOS</h1>
  <hr/>
  {{> yield}}
  <hr/>
  <p></p>
  <a href="/">Home</a>
  <a href="/login">Login</a>
  <a href="/register">Register</a>
</template>

<template name="home">
  <h2>Home</h2>
</template>

<template name="login">
  <h2>Login</h2>
</template>

<template name="register">
  <h2>Registrarse</h2>
</template>
```

### Rutas Dinamicas

`{{ pathFor route='name'}}` Ejemplo en el que si cambiamos el path de la ruta 
el link seguira funcionando  

```javascript
Router.route('/', {
  name: 'home',
  template: 'home'
});

<a href="{{pathFor route='home'}}">Home</a>
```

`{{ urlFOr}}` PENDIENTE  
`{{ linkTo}}` PENDIENTE  

---

## ACCOUNTS-PASSWORD

Crea una coleccion `Meteor.users`  
Nos ofrece el helper `currentUser` que es reactivo y es true si estas logueado 
y false si no se esta logueado  

```sh
Meteor.userId() // nos devuelve el current user Id
this.userId     // dentro de las publicaciones 
                // pues Meteor.userId() no funciona ahi
```

Crear Usuario - Registrarse   

```javascript
Accounts.createUser({
  email: email,
  password: password
}, function (error) {
  if (error) {
    console.log(error.reason);
  } else {
    Router.go('home');
  }
});
// Al registrarse automaticamente te loguea
```

Desloguearse  

```sh
Meteor.logout();
```

Loguearse  
 
```javascript
Meteor.loginWithPassword(email, password, function (error) {
  if (error) {
    console.log(error.reason);
  } else {
    Router.go('home');
  }
});

```

Si solo queremos nombre de usuario, no correo  

```javascript
/client/config.js
Accounts.ui.config({
  passwordSignupFields: 'USERNAME_ONLY'
});
```

`accounts-ui` es una paquete complementario que provee una interfaz grafica 
ya diseñada para loguearse 

```javascript
// Inserta la interfaz de logueo en una plantilla
{{> loginButtons}} 
// para ponerla a la derecha o a la izquierda
{{> loginButtons align="right"}}) 
```

---

## ENLACES

### Documentacion Oficial

`Manuales`    
[Documentacion oficial basica](http://docs.meteor.com/#/basic/)  
[Documentacion oficial completa](http://docs.meteor.com/#/full/)  
[Manual de Meteor](http://manual.meteor.com/)  
[Guia meteor](//guide.meteor.com)  

`Foros`  
[Foros oficiales de Meteor](https://forums.meteor.com/)  

`Moviles`  
[Android e IOS](https://www.meteor.com/tutorials/blaze/running-on-mobile)  
[Subir una app Android a la Play Store](https://github.com/meteor/meteor/wiki/How-to-submit-your-Android-app-to-Play-Store)  
[Integracion de Cordova/Phonegap en Meteor](https://github.com/meteor/meteor/wiki/Meteor-Cordova-Phonegap-integration)   

### Blogs Meteor

`Activos`  
[Just meteor](http://justmeteor.com/blog/)  
[The Meteor Chef](http://themeteorchef.com/)  
[Discover Meteor](https://www.discovermeteor.com/blog)  
[Experiments in Meteor](http://experimentsinmeteor.com/)  
[Meteor Point](http://meteorpoint.com/)  
[Crater.io](https://crater.io/)  
[This week in Meteor](http://thisweekinmeteor.com/)  
[Cajon de sastre Meteor](http://www.smashingthingstogether.com/)  
[Meteorhacks](https://meteorhacks.com/)  
[Blog de Seguridad Meteor](http://blog.east5th.co/)  
[Web tempest](http://www.webtempest.com/)  
[Meteor en Medium.com](https://medium.com/search?q=meteor)  
[David Weldon](https://dweldon.silvrback.com/)  

`Antiguos o Poco activos`  
[Gentlenode](https://gentlenode.com/journal/meteor)  
[Autor de Meteor In Action](http://www.manuel-schoebel.com/blog)   
[Meteor Capture](http://meteorcapture.com/)  
[Meteor elfoslav](http://meteor.hromnik.com/)  
[Yauh](https://www.yauh.de/)  

### Tutoriales

[Meteor Tutorial Book](http://www.meteor-tutorial.org/book)  
[Discover Meteor Español](http://es.discovermeteor.com/)  
[You First Meteor Application](http://meteortips.com/first-meteor-tutorial/)  
[You Second Meteor Application](http://meteortips.com/second-meteor-tutorial/)  
[Meteor Tutorial for Twitter clone](http://randomdotnext.com/meteor-tutorial-p1/)  
[Slack Clone in Meteor](https://scotch.io/tutorials/building-a-slack-clone-in-meteor-js-getting-started)  
[Creacion de escrutinio geolocalizado para elecciones](https://medium.com/@SamCorcos/building-campaignhawk-an-open-source-election-canvassing-app-with-meteor-and-react-part-1-aff5e8e19a32)  
[BulletProof](https://bulletproofmeteor.com/basics/introduction)  
[Meteorpedia.com](http://meteorpedia.com/read/Main_Page)  

### Bases de Datos

`MongoDB`  
[Instalar MondoDB 2.6 en Ubuntu 14.04](http://knowledgepia.com/en/k-blog/database/how-to-install-mongodb-2-6-on-ubuntu-14-10-14-04-and-12-04-lts)  
[Instalar MongoDB 2.6 en Ubuntu/Debian](https://futurestud.io/blog/how-to-install-mongodb-2-6-on-ubuntudebian/)  
[Crear usuarios en MongoDB 2.6](https://gist.github.com/tamoyal/10441108)  
[Crear una Dinamic query var query = {} MongoDB](https://stackoverflow.com/questions/27296885/meteor-collection-find-use-variable-as-field-in-dynamic-mongo-query)  
[Diseño de esquemas de datos en MongoDB](http://blog.mongodb.org/post/87200945828/6-rules-of-thumb-for-mongodb-schema-design-part-1)  

`PostgreSQL`  
[Documentacion oficial de Meteor PostgreSQL](https://meteor-postgres.readthedocs.org/en/latest/)  
[Storeness postgreSql](https://github.com/storeness/meteor-postgres)  
[Numtel postgreSql](https://github.com/numtel/meteor-pg)  

`MySQL`  
[Numtel MySql](https://github.com/numtel/meteor-mysql)  

### Spacebars

[Documentacion oficial Spacebars GitHub](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md)  
[Meteor Capture Spacebars](http://meteorcapture.com/spacebars/)  

### Paquetes

`iron:router`  
[Guia Iron Router](https://iron-meteor.github.io/iron-router/)  

`kadira:flow-router`  
[Guia Flow Router](https://kadira.io/academy/meteor-routing-guide/content/introduction-to-flow-router)  
[Documentacion oficiareywood:publish-compositel GitHub de Flow Router](https://github.com/kadirahq/flow-router)  

`underscore`  
[Documentacion oficial undescore.js](http://underscorejs.org/)  

`meteorhacks:npm`  
[Meteorhacks npm](https://github.com/meteorhacks/npm)    

`reywood:publish-composite`  
[Joins reactivos](http://braindump.io/meteor/2014/09/12/publishing-reactive-joins-in-meteor.html)  

### Deploy

[Compose, Modulus, Digital Ocean](http://meteortips.com/deployment-tutorial/)  
[DO - How To Deploy a Meteor.js Application on Ubuntu 14.04 with Nginx](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-meteor-js-application-on-ubuntu-14-04-with-nginx)  
[Meteor Up - Arunoda](https://github.com/arunoda/meteor-up)  
[A pelo (la que uso)](https://scotch.io/tutorials/building-a-slack-clone-in-meteor-js-part-5-deployment)  

### Moviles

[Build Android APK with MeteorJS Guide](http://kafechew.com/2015/05/23/build-android-apk-with-meteorjs-guide/)  
[A Beginners Guide to Mobile Development with Meteor](http://www.sitepoint.com/beginners-guide-mobile-development-meteor/)  
[Meteor-React-Ionic Mobile App (Guia con muchas partes)](https://medium.com/@SamCorcos/meteor-react-ionic-mobile-app-part-1-the-basic-template-9355ebf3397f)  
[Meteor + Cordova + Mupx problems](http://code.krister.ee/mupx-problems/)  
[Install Android Studio/SDK on Debian Wheezy 64bit](http://techieventures.blogspot.com.es/2014/11/install-android-studiosdk-on-debian.html)  
[Crosswalk Project](https://crosswalk-project.org/)  

### Articulos

[Buenas practicas para grandes projectos en Meteor](https://blog.tableflip.io/large-meteor-projects-best-practices/)  
[Meteorhacks npm best practice](https://forums.meteor.com/t/meteorhacks-npm-best-practice/7918)  
[Is packages-for-everything the solution to all Meteor problems?](https://forums.meteor.com/t/is-packages-for-everything-the-solution-to-all-meteor-problems/1785)  
[Despliegue completo de la app Alpheartz con todas sus vicisitudes](https://medium.com/@MaxDubrovin/alpheratz-meteor-app-deployment-overview-711a25c20adf)  

### Trabajo

[Trabajos para Meteor](http://www.weworkmeteor.com/)  

### Compendio de recursos

[Meteor help](http://meteorhelp.com/)  
[Meteor ZEEF](https://meteor-js.zeef.com/rene.schneider) 

---





