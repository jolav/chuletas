# MODULOS NPM

---

## HANDLEBARS

### instalacion con express

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

```js
const exphbs = require('express-handlebars');
app.engine('.hbs', exphbs({
  defaultLayout: 'main',
  extname: '.hbs',
  layoutsDir: path.join(__dirname, 'views/layouts')
}));
app.set('view engine', '.hbs');
app.set('views', path.join(__dirname, 'views'));
```

* **consolidate**

`npm install --save consolidate`  
`npm install --save handlebars`  
 
```js
var consolidate = require('consolidate');
app.engine('html | hbs | handlebars', consolidate.handlebars);
app.set('view engine', 'html | hbs | handlebars');
app.set('views', __dirname + '/views');
```


---

## ASYNC

[GitHub Async](https://github.com/caolan/async)  
[npm Async](https://www.npmjs.com/package/async)  

![nodejs](/z-static/images/npm/callback1.jpg)

---

## PASSPORT

[Pagina Web Passport](http://passportjs.org)  
[npm Passport](https://www.npmjs.com/package/passport)  

---
