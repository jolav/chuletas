# VARIOS

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

```javascript
// else que se mostrara solo cuando la lista este vacia
{{#each paragraphs}}
  <p>{{this}}</p>
{{else}}
  <p class="empty">No content</p>
{{/each}}

// {{@index}} es el numero de iteracion (indice)
{{#each array}}
  {{@index }}: {{this}}
{{/each}}

//  {{@key}} es el nombre de la clave (del par clave/valor)
{{#each object}}
  {{@key }}: {{this}}
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