# HTML5

---

## ESTRUCTURA

```html
<!DOCTYPE html>
<html>
  <head>
    <title> Titulo </title>
  </head>
  <body>
  </body>
</html>
```
### Block Elements

ELementos que siempre empiezan en una nueva linea

![html1](/z-static/images/html5/blockElements.png)

![html2](/z-static/images/html5/blockElementsList.png)


### Inline Elements

Elementos que aparecen a continuacion en la misma linea del anterior

![html3](/z-static/images/html5/inlineElements.png)

![html4](/z-static/images/html5/inlineElementsList.png)

### Caracteres de escape

![html5](/z-static/images/html5/escapeCharacters.png)

---

## TEXTO

**white space collapsing :** Dos o mas espacios o lineas juntos el navegador 
solo mostrara uno    

* **Elementos para estructurar la pagina**

```html
<p> paragraph </p>
<h1..6> headings </h1..6>
<b> bold </b>
<i> italic </i>
<sup> superscript </sup>
<sub> subscript </sub>
<br /> line breaks
<hr /> horizontal rules
```

* **Elementos semanticos**

```html
<strong> contenido importante </strong>  
<em> enfasis </em>  
<blockquote> citar parrafos </blockquote>  
<q> citas cortas dentro de un parrafo </q>
<abbr> abreviaturas o acronimos </abbr>  
<cite> referencia a algo (libro, peli ...) </cite>  
<dfn> la primera explicacion de un termino en el documento </dfn>  
<address> para contener detalles de contacto de alguien </address>  
<ins> mostrar contenido que ha sido insertado en el documento </ins>  
<del> mostrar contenido que ha sido borrado en el documento </del>  
<s> indicar que algo ya no es relevante</s>  
```

---

## LISTAS

* **Ordenadas**

```html
<ol>
  <li>primero</li>
  <li>segundo</li>
  <li>tercero</li>
</ol>
```
<ol>
  <li>primero</li>
  <li>segundo</li>
  <li>tercero</li>
</ol>

* **Desordenadas**

```html
<ul>
  <li>primero</li>
  <li>segundo</li>
  <li>tercero</li>
</ul>
```
<ul>
  <li>primero</li>
  <li>segundo</li>
  <li>tercero</li>
</ul>

* **Listas de deficiones**

```html
<dl>
  <dt> concepto 1</dt>
  <dd> definicion de concepto 1</dd>   
  <dt> concepto 2</dt>
  <dd> definicion de concepto 2</dd>        
  <dt> concepto 3</dt>
    <dd> definicion de concepto 3</dd>
</dl>
```
<dl>
  <dt> concepto 1</dt>
  <dd> definicion de concepto 1</dd>   
  <dt> concepto 2</dt>
  <dd> definicion de concepto 2</dd>        
  <dt> concepto 3</dt>
  <dd> definicion de concepto 3</dd>
</dl>

---

## ENLACES

* **A paginas externas**

```html
<a href="http://www.brusbilis.com">href como url absoluta</a>
```

* **A paginas del mismo sitio**

```html
<a href="../info/contacto.html">Contacto href como url relativa</a>
```

* **A direcciones de correo**

```html
<a href="mailto:info@brusbilis.com">Mandar correo</a>
```

* **A una nueva pestaña**

```html
<a href="http://www.imdb.com" target="_blank">Movie Database</a>
```

* **A una parte de la misma pagina**

```html
<h1 id="top">Bla bla bla</h1>
...
...
<a href="#top">Subir</a>
```

* **A una parte de otra pagina**

```html
<a href="URL relativa o absoluta/#top">Enlace</a>
```

---

## IMAGENES

Ponerlas todas en una carpeta `/imagenes`  
Salvarlas a resolucion de 72 ppi(pices per inch), los monitores no dan mas y asi ahorramos peso en la imagen  

```html
<img src="URL relativa o absoluta a la imagen" />
```

```html
<img alt="lo que se lee si no se carga la imagen" />
```

```html
<img title="descripcion de la imagen, sale al poner el cursor encima" />
```

CODIGO VIEJO -Altura ,anchura y alineamiento de la imagen es mejor hacerlo por 
CSS  

```html
<img src="images/imagen.jpg" alt="loquesea" width="600" height="450" />

<img src="imagen.png" alt="alt" align="left|right|top|bottom|middle" />
```

`<figure>` se usa para tener juntas y asociar las imagenes con sus
pies de fotos `<figcaption>`

Pero tambien se puede usar para contener

* imagenes
* videos
* graficos
* diagramas
* codigo
* texto

```html
<figure>
  <img src="imagen.jpg" alt="loquesea">
  <br />
  <figcaption>Pie de foto</figcaption>
</figure>
```

---

## TABLAS

`<table>` para crear la tabla  
`<tr>` fila  
`<td>` celda  
`<td colspan="2">Extiende el texto a dos columnas</td>`  
`<td rowspan="2">Extiende el texto a dos filas</td>`  
`<th>` es una celda pero cabecera de la fila o columna  
>`scope="row" scope="col"` atributo que indica si es fila o columna  

`<thead> <tbody> <tfoot>`

CODIGO VIEJO (Ahora por CSS)
`width spacing bgcolor border`

```html
<table>
  <thead>
    <tr>
      <th></th>
      <th scope="col">Sabado</th>
      <th scope="col">Domingo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Cervezas</th>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th scope="row">Cervezas</th>
      <td>3</td>
      <td>3</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th scope="row">Total</th>
      <td>7</td>
      <td>5</td>
    </tr>
  </tfoot>
</table>
```

---

## FORMULARIOS

`action`  
`method`  
`id`  
`size` CODIGO VIEJO no deberia usarse, para el tamaño del cuadro. Usar CSS
`type="text"` crea una entrada de texto de una sola linea  
`type="password"`  como text pero los caracteres no se ven al teclearlos  
`name`  nombre de la variable, el valor sera lo introducido en el formulario  
`maxlength` limitar los caracteres, p ej mes 2 caracteres  
`type="submit"` boton que envia el formulario, el value es el texto que sale en el boton  
`placeholder` en cualquier input de texto es el valor que saldra por defecto hasta que el
usuario lo pinche encima

```html
<form method="get or post" action="url del servidor que va a responder">
  Usuario<input type="text" name="usuario" />
  Contraseña<input type="password" name="contrasena" />    
  <input type="submit" value="Lo que sale en el boton para enviar" />
</form>
```

### Elementos  

* **`textarea`**

cols y rows son las dos CODIGO VIEJO, mejor con CSS

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <textarea name="comments" cols="20" rows="4">Escribir Aqui...
  </textarea>
</form>

```html
<textarea name="sugerencias" cols="40" rows="3">Escribir Aqui...
</textarea>
```

* **`radio button`**  

Puedes elegir una opcion

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Elige una opcion <br />
    <input type="radio" name="opcion" value="1" /> Opcion 1
    <input type="radio" name="opcion" value="2" checked="checked"/> Opcion 2
    <input type="radio" name="opcion" value="3" /> Opcion 3
  </p>
</form>  

```html
<form action="http://rutaquegestionaelform">
  <p>Elige una opcion <br />
    <input type="radio" name="opcion" value="1" /> Opcion 1
    <input type="radio" name="opcion" value="2" 
      checked="checked"/> Opcion 2
    <input type="radio" name="opcion" value="3" /> Opcion 3
  </p>
</form>
```

* **`checkbox`**

Pues elegir cero, una o mas opciones de las disponibles  

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Elige una opcion <br />
    <input type="checkbox" name="opcion" value="1" checked="checked" /> Opcion 1
    <input type="checkbox" name="opcion" value="2" /> Opcion 2
    <input type="checkbox" name="opcion" value="3" checked="checked" /> Opcion 3
  </p>
</form>  

```html
<form action="http://rutaquegestionaelform">
  <p>Elige una opcion <br />
    <input type="checkbox" name="opcion" value="1" checked="checked" /> 
      Opcion 1
    <input type="checkbox" name="opcion" value="2" /> Opcion 2
    <input type="checkbox" name="opcion" value="3" checked="checked" /> 
      Opcion 3
  </p>
</form>
```

* **`select : drop down list`**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Opcion a elegir ?</p>
  <select name="opciones">
    <option value="1">Opcion 1</option>
    <option value="2" selected="selected" >Opcion 2</option>
    <option value="3">Opcion 3</option>
  </select>
</form>  

```html
<form action="http://rutaquegestionaelform">
  <p>Opcion a elegir ?</p>
  <select name="opciones">
    <option value="1">Opcion 1</option>
    <option value="2" selected="selected">Opcion 2</option>
    <option value="3">Opcion 3</option>
  </select>
</form>
```

* **`multiple select box`**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Opcion a elegir ?</p>
  <select name="opciones"size="3" multiple="mutiple">
    <option value="1">Opcion 1</option>
    <option value="2" selected="selected" >Opcion 2</option>
    <option value="3">Opcion 3</option>
    <option value="4">Opcion 4</option>
    <option value="5">Opcion 5</option>
  </select>
</form>  

```html
<form action="http://rutaquegestionaelform">
  <p>Opcion a elegir ?</p>
  <select name="opciones"size="3" multiple="mutiple">
    <option value="1">Opcion 1</option>
    <option value="2" selected="selected" >Opcion 2</option>
    <option value="3">Opcion 3</option>
    <option value="4">Opcion 4</option>
    <option value="5">Opcion 5</option>
  </select>
</form>
```  

* **`file input box`**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Subir un archivo</p>
  <input type="file" name="nombreArchivo" /><br />
  <input type="submit" value="Upload" />
</form>  

```html
<form action="http://rutaquegestionaelform" method="post">
  <p>Subir un archivo</p>
  <input type="file" name="nombreArchivo" /><br />
  <input type="submit" value="Upload" />
</form>
```

* **`image button`**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios" >
  <p>Texto de lo que sea</p>
  <input type="text" name="email" />
  <input type="image" src="images/subscribe.jpg" width="100" height="20" />
</form>

```html
<form action="http://rutaquegestionaelform" >
  <p>Texto de lo que sea</p>
  <input type="text" name="email" />
  <input type="image" src="/ruta/imagen.jpg" width="100" height="20" />
</form>
```

* **`button and hidden controls`**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <button>
    <img src="images/add.gif" alt="add" width="10" height="10" /> Add
  </button>
  <input type="hidden" name="nombreQueQueramos" value="valorAPasar" />
</form>  

```html  
<form action="http://rutaquegestionaelform" >
  <button>
    <img src="images/add.gif" alt="add" width="10" height="10" /> Add
  </button>
  <input type="hidden" name="nombreQueQueramos" value="valorAPasar" />
</form>  
```

* **`labeling form controls`**

```html
uso de etiquetas <label></label> para accesibilidad
```

* **`grouping form elements`**

<fieldset>
  <legend>Detalles contacto</legend>
  <label>Email:<br />
    <input type="text" name="email" />
  </label><br />
  <label>Movil:<br />
    <input type="text" name="mobile" />
  </label><br />
  <label>Telefono fijo:<br />
    <input type="text" name="telephone" />
  </label>
</fieldset>  

```html
<fieldset>
  <legend>Detalles contacto</legend>
  <label>Email:<br />
    <input type="text" name="email" />
  </label><br />
  <label>Movil:<br />
    <input type="text" name="mobile" />
  </label><br />
  <label>Telefono fijo:<br />
    <input type="text" name="telephone" />
  </label>
</fieldset>
```

### Validacion

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  Usuario<input type="text" name="usuario" required="required"/> <br />
  Contraseña<input type="password" name="contrasena" required /> <br />
  <input type="submit" value="Enviar" />
</form>

```html
<form action="http://rutaquegestionaelform" >
  Usuario<input type="text" name="usuario" required="required"/> <br />
  Contraseña<input type="password" name="contrasena" required /> <br />
  <input type="submit" value="Enviar" />
</form>
```

### Tipos de INPUT

* **Fecha**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Elige Fecha</p>
  <input type="date" name="date" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="date">
```

* **Correo**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Correo</p>
  <input type="email" name="email" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="email">
```

* **URL**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>URL</p>
  <input type="url" name="url" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="url">
```

* **Telefono**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Telefono</p>
  <input type="tel" name="tel" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="tel">
```

* **Numero**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Numero</p>
  <input type="number" name="number" min="10" max="20" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="number">
```

* **Range**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Numero</p>
  <input type="range" name="range" min="0" max="20" step="4" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="range">
```

* **Color**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Color</p>
  <input type="color" name="color" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="color">
```

* **Search**

<form action="http://brusbilis.com/chuletas/frontend/html5/#formularios">
  <p>Search</p>
  <input type="search" name="search" />
  <input type="submit" value="Enviar" />
</form>

```html
<input type="search">
```

---

## VIDEO Y AUDIO

### Video

<video src="html5docs/videos/SampleVideo_1280x720_1mb.mp4"
  poster="/images/html5/OkDedo.jpg"
  width="400" height="300"
  preload
  controls
  loop>
  <p>Descripcion del video</p>
</video>

```html
<video src="html5docs/videos/SampleVideo_1280x720_1mb.mp4"
  poster="/images/html5/OkDedo.jpg"
  width="400" height="300"
  preload
  controls
  autoplay
  loop>
  <p>Descripcion del video</p>
</video>
```

Para poner diferentes formatos usamos `<source>`

```html
<video poster="images/puppy.jpg" width="400" height="320" >
  <source src="video.mp4" type='video/mp4;
    codecs="avc1.42E01E, mp4a.40.2"' />
  <source src="video.webm" type='video/webm;codecs="vp8, vorbis"' />
  <p>Descripcion del video</p>
</video>
```

### Audio

<audio src="html5docs/music/ACDC.ogg" controls>
  <p>Formato no soportado por el navegador</p>
</audio>

```html
<audio src="html5docs/music/ACDC.ogg" controls autoplay preload loop>
  <p>Formato no soportado por el navegador</p>
</audio>
```

Para poner diferentes formatos usamos `<source>`

```html
<audio controls autoplay>
  <source src="music/nombre.ogg" />
  <source src="music/nombre.mp3" />
  <p>Formato no soportado por el navegador</p>
</audio>
```

---

## HTML5 LAYOUT

<img src="/z-static/images/html5/oldLayout.png" alt="html6" width="320"/>
<img src="/z-static/images/html5/newLayout.png" alt="html6" width="320"/>


* **header, footer, nav**

<header>
  <h4>Yoko's Kitchen</h4>
  <nav>
    <ul>
      <li><a href="" class="current">home</a></li>
      <li><a href="">classes</a></li>
      <li><a href="">catering</a></li>
      <li><a href="">about</a></li>
      <li><a href="">contact</a></li>
    </ul>
  </nav>
</header>
<footer>
	 &copy; 2011 Yoko's Kitchen
</footer>

```html
<header>
  <h4>Yoko's Kitchen</h4>
  <nav>
    <ul>
      <li><a href="" class="current">home</a></li>
      <li><a href="">classes</a></li>
      <li><a href="">catering</a></li>
      <li><a href="">about</a></li>
      <li><a href="">contact</a></li>
    </ul>
  </nav>
</header>
<footer>
	 &copy; 2011 Yoko's Kitchen
</footer>
```

* **article**

<article>
  <hgroup>
    <h2>Japanese Vegetarian</h2>
    <h3>Five week course in London</h3>
  </hgroup>
  <p>A five week introduction to traditional Japanese vegetarian meals,
   teaching you a selection of rice and noodle dishes.</p>
</article>
<article>
  <hgroup>
    <h2>Sauces Masterclass</h2>
    <h3>One day workshop</h3>
  </hgroup>
  <p>An intensive one-day course looking at how to create the most 
    delicious sauces for use in a range of Japanese cookery.</p>
</article>

```html
<article>
  <hgroup>
    <h2>Japanese Vegetarian</h2>
    <h3>Five week course in London</h3>
  </hgroup>
  <p>A five week introduction to traditional Japanese vegetarian 
    meals, teaching you a selection of rice and noodle dishes.</p>
</article>
<article>
  <hgroup>
    <h2>Sauces Masterclass</h2>
    <h3>One day workshop</h3>
  </hgroup>
  <p>An intensive one-day course looking at how to create the most 
    delicious sauces for use in a range of Japanese cookery.</p>
</article>
```

* **aside, section**

<aside>
  <section class="popular-recipes">
    <h3>Popular Recipes</h3>
    <a href="">Yakitori (grilled chicken)</a><br/>
    <a href="">Tsukune (minced chicken patties)</a><br/>
    <a href="">Okonomiyaki (savory pancakes)</a><br/>
    <a href="">Mizutaki (chicken stew)</a><br/>
  </section>
  <section class="contact-details">
    <h3>Contact</h3>
    <p>Yoko's Kitchen<br />
    27 Redchurch Street<br />
    Shoreditch<br />
    London E2 7DP</p>
  </section>
</aside>


```html
<aside>
  <section class="popular-recipes">
    <h3>Popular Recipes</h3>
    <a href="">Yakitori (grilled chicken)</a><br/>
    <a href="">Tsukune (minced chicken patties)</a><br/>
    <a href="">Okonomiyaki (savory pancakes)</a><br/>
    <a href="">Mizutaki (chicken stew)</a><br/>
  </section>
  <section class="contact-details">
    <h3>Contact</h3>
    <p>Yoko's Kitchen<br />
    27 Redchurch Street<br />
    Shoreditch<br />
    London E2 7DP</p>
  </section>
</aside>
```

* **hgroup**

<hgroup>
  <h3>Japanese Vegetarian</h3>
  <h4>Five week course in London</h4>
</hgroup>

```html
<hgroup>
  <h3>Japanese Vegetarian</h3>
  <h4>Five week course in London</h4>
</hgroup>
```

* **figure, figcaption**

`<figure>` se usa para tener juntas y asociar las imagenes con sus
pies de fotos `<figcaption>`

Pero tambien se puede usar para contener

* imagenes
* videos
* graficos
* diagramas
* codigo
* texto

```html
<figure>
  <img src="images/bok-choi.jpg" alt="Bok Choi" />
  <figcaption>Bok Choi</figcaption>
</figure>
```

* **div**

`<div>` queda para todo lo que no sea claramente algo de lo anterior y para
tener juntos elementos relacionados o envolver partes de la pagina

```html
<div class="wrapper">
  <header>
    <h1>Yoko's Kitchen</h1>
    <nav><!-- nav content here -->
    </nav>
  </header>
    <section class="courses"><!-- section content here -->
    </section>
    <aside><!-- aside content here -->
    </aside>
  <footer><!-- footer content here -->
  </footer>
</div><!-- .wrapper -->
```

* **Convertir bloque en enlace**

Envolvemos el elemento a converir en enlace con `<a>`

```html
<a href="introduction.html">
  <article>
    <figure>
      <img src="images/bok-choi.jpg" alt="Bok Choi" />
      <figcaption>Bok Choi</figcaption>
    </figure>
    <hgroup>
      <h2>Japanese Vegetarian</h2>
      <h3>Five week course in London</h3>
    </hgroup>
    <p>A five week introduction to traditional Japanese vegetarian meals,
      teaching you a selection of rice and noodle dishes.</p>
  </article>
</a>
```

---
