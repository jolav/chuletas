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

![html1](/_img/html-css/blockElements.png)

![html2](/_img/html-css/blockElementsList.png)


### Inline Elements

Elementos que aparecen a continuacion en la misma linea del anterior

![html3](/_img/html-css/inlineElements.png)

![html4](/_img/html-css/inlineElementsList.png)

### Caracteres de escape

![html5](/_img/html-css/escapeCharacters.png)

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
<h1 id="top">Bla bla bla</h1> ó <h1 name="top">Bla bla bla</h1>
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
Salvarlas a resolucion de 72 ppi(pixels per inch), los monitores no dan mas y asi ahorramos peso en la imagen  

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

<img src="/_img/html-css/oldLayout.png" alt="html6" width="320"/>
<img src="/_img/html-css/newLayout.png" alt="html6" width="320"/>


* **header, footer, nav, article, aside, section**

* **hgroup, figure, figcaption**

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


---

## INTRODUCCION

* **Enlaces a css**

Externo

```html
<head>
  <title>CSS Externo</title>
  <link href="css/estilo.css" type="text/css" rel="stylesheet" />
</head>
```

Si hay multiples hojas de estilo :

* `<link>` uno para cada hoja de estilo
* En la hoja de estilo que se enlaza incluir `@import url("estilo2.css);"`

Interno - NO USAR

```html
<head>
  <title>CSS Interno</title>
  <style type="text/css">
    body {
      font-family: calibri;
      background-color: green;}
    h1 {
      color: black;}
  </style>
</head>
```

### Selectores

* **Selectores CSS**

![css1](/_img/html-css/cssSelectors.png)

* **Selectores de atributos**

![css2](/_img/html-css/atributeSelectors.png)

### Cascada y herencia

* **Cascada**

\- Ultima Regla: la ultima regla aplicada es la que manda  
\- `!important` añadiendo esto despues de la regla asi toma prioridad
```css
  color: blue !important;
```
\- Especificidad : prevalecera la regla del selector que sea mas especifico  
> `h3` es mas especifico que `*`  
> `p otro` es mas especifico que `p`  
> `p#intro` es mas especifico que `p`  

* **Herencia**

Casi todos los selectores anidados dentro de otros heredan las propiedades
asignadas al selector exterior. Por ejemplo un color en body tambien se
aplicara al texto de un parrafo.  

Hay excepciones evidentes como margin-top (un parrafo no tiene el mismo margen
superior que el body del documento).  

Puedes forzar heredar valores de propiedades de los padres usando `inherit` como
valor de la propiedad

```css
body {  
  padding: 20px;}  
.pagina {  
  padding: inherit;}  
```  

---

## COLOR

`color` -  

```css
h1 {
	 color: DarkCyan;}                 /* color name */
h2 {
	 color: #ee3e80;}                  /* hex code */
p {
	 color: rgb(100,100,90);}          /* rgb value */
```

`background color`-

```css
body {
	 background-color: rgb(150,150,150); }
h1 {
	 background-color: DarkCyan; }
h2 {
	 background-color: ##ee3e80: }
p {
	 background-color: red; }
```

`opacity`, `rgba`-  

```css
p.one {
	 background-color: rgb(0,0,0);
	 opacity: 0.5;}
p.two {
	 background-color: rgb(0,0,0);
	 background-color: rgba(0,0,0,0.5);}
```

---

## TEXTO

`cursor` - Controla el tipo de cursor del raton que se muestra
`auto|crosshair|default|pointer|move|text|wait|help|url("cursor.png")`  

```css
a {
  cursor: move;}
```

`font-family` - Familias : `serif`, `sans-serif`, `cursive`, `fantasy` y `monospace`  

```css
font-family: Arial, Verdana, sans-serif
```

`font-size` -

* Pixel: Por defecto en los navegadores el texto es de 16px
* Porcentaje : porcentaje sobre los 16px estandar, p.ej 75% es 12px
* em : unidad equivalente a la anchura de la m

`font-face` - 

```css
@font-face {
  font-family: 'ChunkFiveRegular';
  src: url('fonts/chunkfive.eot');
  src: url('fonts/chunkfive.eot?#iefix')	format('embedded-opentype'),
    url('fonts/chunkfive.woff') format('woff'),
    url('fonts/chunkfive.ttf') format('truetype'),
    url('fonts/chunkfive.svg#ChunkFiveRegular') format('svg');}
```

`font-weight` - `normal|bold`  

`font-style` - `normal|italic|oblique`  

`text-transform`- 

* uppercase : convierte el texto a mayusculas
* lowercase : convierte el texto a minusculas
* capitalize : pone mayusculas la primera letra de cada palabra

```css
h1 {
  text-transform: uppercase;}
```

`text-decoration`- 

* none : elimina cualquier decoracion del texto
* underline : subraya el texto
* overline : coloca una linea por encima del texto
* line-through : linea tachando el texto
* blink : hace que el texto brille y deje brillar

```css
a {
	 text-decoration: none;}
```

`line-height`- distancia entre lineas  

```css
p {
  line-height: 1.4em; }
```

`letter-spacing`, `word-spacing` - distancia entre letras, distancia entre palabras, por defecto es 0.25em  

```css
h1, h2 {
  text-transform: uppercase;
  letter-spacing: 0.2em; }
.credits {
  font-weight: bold;
  word-spacing: 1em; }
```

`text-align`- 

* left : el texto se alinea a la izquierda
* right : el texto se alinea a la derecha
* center : el texto se centra
* justify : Cada linea excepto la ultima ocupa toda la anchura disponible

```css
h1 {
  text-align: left; }
```

`vertical-align`- 

Usada con elementos en linea como `<img>` `<em>` `<strong>` y es muy similar al
atributo `align` de `<img>`

Valores :
`baseline`, `sub`, `super`, `top`, `text-top`, `middle`, `bottom`, `text-bottom`

![css3](/_img/html-css/verticalAlignExample.png)

```css
#six-months {
  vertical-align: text-top;}
#one-year {
  vertical-align: baseline;}
#two-years {
  vertical-align: text-bottom;}
```

`text-indent`- indenta el texto  

```css
h2 {
  text-indent: 20px; }
```

`text-shadow` - 

1. distancia a izda o dcha de la sombra
2. distancia arriba o abajo de la sombra
3. (opcional) cantidad de difuminado de la sombra
4. color de la sombra

```css
p {
  text-shadow: 2px 2px 1px #222222}
```

`:first-letter`, `:first-line` (pseudo-elementos) -  Para especificar diferentes valores a la primera letra o primera linea del texto    

```css
p.intro:first-letter {
  font-size: 200%;}
p.intro:first-line {
  font-weight: bold;}
```

`:link`, `:visited` (pseudo-clases) - Los navegadores muestran los enlaces en azul y subrayados por defecto y cambian el color cuando han sido visitados  

* :link : para poner estilos a enlaces no visitados
* :visited : para poner estilos a enlaces visitados

```css
a:link {
  color: deeppink;
  text-decoration: none;}
a:visited {
  color: black;}
a:hover {
  color: deeppink;
  text-decoration: underline;}
a:active {
  color: darkcyan;}
```

`:hover`, `:active`, `:focus` (pseudo-clases) - 

* :hover : se aplica cuando el raton esta encima (no funciona en tactiles)
* :active : se aplica cuando se pulsa algo
* :focus : se aplica cuando el elemento tiene el foco

```css
cajetinTexto {
  padding: 6px 12px 6px 12px;
  border: 1px solid #665544;
  color: #ffffff;}
boton.submit:hover {
  background-color: #665544;}
boton.submit:active {
  background-color: chocolate;}
cajetinTexto.text {
  color: #cccccc;}
cajetinTexto.text:focus {
  color: #665544;}
```

---

## BOXES

### Dimensiones

`width`, `height`- Por defecto las cajas tienen el tamaño justo para contener a su elemento. Podemos establecer nosotros ese tamaño usando  `pixeles`, o `porcentajes` relativos a la ventana del navegador o a la caja en que ya esta
si esta dentro de otra caja, o `ems`    

```css
div.box {
  height: 300px;
  width: 300px;
  background-color: #bbbbaa;}
```

`min-width`, `max-width` - Minimo y maximo anchura que una caja de un elemento puede tener  

`min-height`, `max-height`- Minimo y maximo anchura que una caja de un elemento puede tener  

`overflow`- Indica como actuar si el contenido dentro de la caja es mayor que la caja.  

* hidden : el contenido extra que no cabe sencillamente se esconde  
* scroll : aparece un scroll para acceder a ese contenido extra

```css
p.one {
  overflow: hidden;}
p.two {
  overflow: scroll;}
```

### Visibilidad

`display`- Permite convertir un elemento en linea en un bloque o al reves. Tambien vale para esconder un elemento en la pagina  

* inline : convierte un elemento de bloque en un elemento en linea
* block : convierte un elemento en linea en un elemento de bloque
* inline-block : permite a un elemento de bloque fluir como un elemento en
linea pero manteniendo las caracteristicas de un elemento de bloque
* none : esconde el elemento de la pagina

```css
li {
  display: inline;
  margin-right: 10px;}
li.coming-soon {
  display: none;}
```

`visibility`- Permite esconder cajas pero deja un espacio donde se supone que esta  

* hidden : esconde el elemento
* visible : muestra el elemento

```css
li.coming-soon {
	 visibility: hidden;}
```

`box-shadow`- 

1. distancia a izda o dcha de la sombra
2. distancia arriba o abajo de la sombra
3. (opcional) cantidad de difuminado del borde. Con cero la sombra es una linea
solida
4. Extension de la sombra, positiva hacia fuera, negativa hacia dentro

```css
p {
  box-shadow: 2px 2px 1px 5px #777777}
```

### Bordes

`border-padding-margin`- 

![css4](/_img/html-css/borderPaddingMargin.png)

`border-width` - espesor de la linea de borde `thin|medium|thick`.  
`border-top-width`, `border-right-width`, `border-bottom-width`  `border-left-width` en orden de las agujas del reloj empezando por top   

```css
p.one {
  border-width: 2px;}
p.two {
  border-width: thick;}
p.three {
  border-width: 1px 4px 12px 4px;}
```

`border-style`- define el tipo de borde `solid|dotted|dashed|double|groove|ridge|inset|outset|hidden/none`.  
`border-top-style ...` - Puedes elegir bordes individuales  

```css
p.one {border-style: dashed;}
```

`border-color` - elegir color para el borde  
`border-top-color ...` - Tambien se puede elegir el color para cada borde  

```css
p.one {
  border-color: #0088dd;}
p.two {
  border-color: #bbbbaa blue orange #0088dd;}
```

`border-image`- Permite usar imagenes como bordes de las cajas  

`border-radius` -  
`border-top-right-radius` -  
`border-bottom-right-radius` -   
`border-bottom-left-radius` -  
`border-top-left-radius` -  

```css
p {
  border-radius: 5px, 10px, 5px, 10px; }
p.one {
  border-radius: 80px 50px; }
p.two  {
  border-top-left-radius: 80px 50px;}
```

### Margenes

`margin` - Espacio entre el borde y la caja. Lo normal en px aunque tambien se puede en ems o porcentajes.  
Si dos cajas se solapan usaremos el mayor de los dos margenes y el otro no  
Tambien individualmente para cada lado `margin-top, margin-right ...`  

```css
p.example1 {
  margin: 10px 2px 10px 5px;
p.example2 {
  margin: 10px;}
```

`padding`- Espacio entre el borde y el contenido. Lo normal es en px pero se pueden usar ems o porcentajes (% de la ventana del navegador o de la caja donde este contenido el elemento).  
Existen tambien individualmente para cada lado `padding-top, padding-right ...`

```css
p.example1 {
  padding: 10px 2px 10px 5px;
p.example2 {
  padding: 10px;}
```

`centrando contenido` - Para centrar una caja en la pagina ( o centrarla dentro del elemento en el que esta  
> 1. Establecer una `width` para la caja (sino tomara toda la anchura de la
> pagina)
> 2. Poner `margin-left: auto` y `margin-rigth: auto`

```css3
p {
  width: 300px;
  padding: 50px;
  border: 20px solid #0088dd;}
p.example {
  margin: 10px auto 10px auto;
```

---

## LISTAS

`list-style` - Permite declaras las siguientes opciones `type, imagen y position` en cualquier orden.  

```css
ul {
  list-style: inside circle; }
```

`list-style-type` - Para poner el estilo al marcador de las listas  

\- Listas ordenadas `decimal|decimal-leading-zero|lower-alpha|upper-alpha|lower-roman|upper-roman`  
\- Listas desordenadas `none|disc|circle|square`

```css
ol {
  list-style-type: lower-roman;}
```

`list-style-imagen` - Usar una imagen como marcador de listas. Se puede usar en `<ul>` y `<li>`  

```css
ul {
  list-style-image: url("images/star.png");}
```

`list-style-position` - Por defecto el marcador de lista aparece `outside` del indentado de la lista. Podemos cambiarlo por `inside` para que el marcador entre en la caja del texto el cual esta indentado  

```css
ul.t1 {
  list-style-position: outside;}
ul.t2 {
  list-style-position: inside;}
```

---

## TABLAS

Consejos:

* Diferenciar cabeceras en negrita, mayusculas o algo mas
* Oscurecer filas alternativas
* Columnas con numeros alinearlos a la derecha

`width` : poner la anchura de la tabla `<table>`  
`padding` : de las celdas `<th> y <td>`  
`text-transform` : en las cabeceras `<th>`  
`letter-spacing, font-size` : para diferenciar el texto de las cabeceras `<th>`  
`border-top, border-bottom` : para fiferenciar los bordes de la cabecera `<th>`  
`text-align `: para alinear el texto de las celdas a conveniencia `<th> <td>`  
`background-color` : cambiar el color de fondo alternando filas  
`:hover` : iluminar la fila con el puntero encima  

```css
body {
  font-family: Arial, Verdana, sans-serif;
  color: #111111;}
table {
  width: 600px;}
th, td {
  padding: 7px 10px 10px 10px;}
th {
  text-transform: uppercase;
  letter-spacing: 0.1em;
  font-size: 90%;
  border-bottom: 2px solid #111111;
  border-top: 1px solid #999;
  text-align: left;}
tr.even {
  background-color: #efefef;}
tr:hover {
  background-color: #c3e6e5;}
.money {
  text-align: right;}
```

`empty-cells` - `show|hide|inherit` indicar en las celdas vacias si mostramos los bordes o no o hereda el comportamiento de la tabla que le contiene   

```css
table.one {
  empty-cells: show;}
table.two {
  empty-cells: hide;}
```

`border-spacing` ajusta la distancia entre celdas adyacentes, por defecto es
minima. Se pone en px. Si pones dos valores son la separacion horizontal y la
vertical  
`border-collapse` los bordes se juntan en un solo borde

```css
table.one {
  border-spacing: 5px 15px;}
table.two {
  border-collapse: collapse;}
```

---

## FORMULARIOS

* **Dar estilo a entradas de texto**

```css
input {
  font-size: 120%;                // tamaño del texto 
  color: #5a5854;                 // color del texto
  background-color: #f2f2f2;      // color del fondo de la entrada 
  border: 1px solid #bdbdbd;      // marca los bordes de la entrada 
  border-radius: 5px;             // redondea esos bordes
  padding: 5px 5px 5px 30px;
  background-repeat: no-repeat;
  background-position: 8px 9px;
  display: block;
  margin-bottom: 10px;}
input:focus {                     // cambia el color de fondo 
  background-color: #ffffff;
  border: 1px solid #b1e1e4;}
input#email {
  background-image: url("images/email.png");}
input#twitter {
  background-image: url("images/twitter.png");}
input#web {
  background-image: url("images/web.png");}
```

![css5](/_img/html-css/textInputs.png)

* **Dar estilo a botones submit**

```css
input#submit {
  color: #444444;                     // color del texto del boton
  text-shadow: 0px 1px 1px #ffffff;   // apariencia 3d
  border-bottom: 2px solid #b2b2b2;   // para el borde del boton
  background-color: #b9e4e3;
  background: -webkit-gradient(linear, left top,
    left bottom, from(#beeae9), to(#a8cfce));
  background:
    -moz-linear-gradient(top, #beeae9, #a8cfce);
  background:
    -o-linear-gradient(top, #beeae9, #a8cfce);
  background:
    -ms-linear-gradient(top, #beeae9, #a8cfce);}
input#submit:hover {                // cambia la apariencia del boton 
  color: #333333;                   // al pasar por encima
  border: 1px solid #a4a4a4;
  border-top: 2px solid #b2b2b2;
  background-color: #a0dbc4;
  background: -webkit-gradient(linear, left top,
    left bottom, from(#a8cfce), to(#beeae9));
  background:
    -moz-linear-gradient(top, #a8cfce, #beeae9);
  background:
    -o-linear-gradient(top, #a8cfce, #beeae9);
  background:
    -ms-linear-gradient(top, #a8cfce, #beeae9);}
```

![css6](/_img/html-css/submitButton.png)

* **Poner estilo a fieldsets y legends**

```css
fieldset {
  width: 350px;               // controlar el tamaño del form
  border: 1px solid #dcdcdc;
  border-radius: 10px;
  padding: 20px;
  text-align: right;}
legend {
  background-color: #efefef;  // color de fondo
  border: 1px solid #dcdcdc;
  border-radius: 10px;
  padding: 10px 20px;
  text-align: left;
  text-transform: uppercase;}
```

![css7](/_img/html-css/fieldset.png)


* **Alinear formularios**

```css
div {
  border-bottom: 1px solid #efefef;
  margin: 10px;
  padding-bottom: 10px;
  width: 260px;}
.title {
  float: left;
  width: 100px;
  text-align: right;
  padding-right: 10px;}
.radio-buttons label {
  float: none;}
.submit {
  text-align: right;}
```

![css8](/_img/html-css/aligningForm.png)

---

## LAYOUT


### Tipos posicionamiento  

`position: static` normal o estatico es el usado por defecto. Los elementos de bloque se ponen uno a continuacion de otro ocupando todo el ancho de la ventana del navegador salvo que lo limite con `width.  

`position: relative` - Los elementos se posicionan en relacion a donde deberian estar en posicion normal.  
Los desplazamos dando valor a `top, bottom, left, right`. Estos valores se ponen en px, porcentajes o ems  

```css
p.example {
  position: relative;
  top: 10px;
  left: 100px;}
```

`position: absolute` - El elemento se posiciona de forma absolita respecto a su elemento contenedor y el resto de elementos de la pagina le ignoran  

```css
h1 {
  position: absolute;
  top: 0px;
  left: 500px;
  width: 250px;}
```

`position: fixed` - Como el absoluto pero el elemento ahora es inamovible, su posicion permanece igual independiente del resto de elemntos e incluso si se sube o baja en la ventana del navegador.  
Util para hacer menus fijos por ejemplo en la parte superior de la ventana  

```css
h1 {
  position: fixed;
  top: 0px;
  left: 50px;
  padding: 10px;
  margin: 0px;
  width: 100%;
  background-color: #efefef;}
```

`position: inherit`, hereda la posicion del padre  

`float: left|right` - Desplaza los elementos todo lo que puede hacia la dcha o izda segun la propiedad `float`.   
Cualquier otro elemento dentro del elemento contenedor fluira alrededor del
elemento que flota  

```css
blockquote {
   float: right;
   width: 275px;
   font-size: 130%;
   font-style: italic;
   font-family: Georgia, Times, serif;
   margin: 0px 0px 10px 10px;
   padding: 10px;
   border-top: 1px solid #665544;
   border-bottom: 1px solid #665544;}
```

* **propiedades de apoyo**

`clear: left|right|both|none` - indica que ningun otro elemento del mismo
contenedot debe tocar el lado indicado de ese elemento  

```css
.clear {
  clear: left;}
```

`overflow: auto; width:100%` - Si todos los elemento de un contenedor son flotantes puede hacer problemas en ciertos navegadores a la hora de mostrar los bordes  

```css
div {
  border: 1px solid #665544;
  overflow: auto;
  width: 100%;}
```

`multicolumna` - Crear diseños multicolumna con floats. Usar un `<div>` para cada columna y con las siguientes propiedades `width`, `float` y `margin` para cerrar un hueco entre las columnas  

`z-index` - Al usar posicionamiento relativo, fijo o absoluto los elementos se pueden solapar. Si es asi el ultimo que aparece en el codigo HTML es el que se ve.  
Para controlar eso esta la propiedad `z-index`, se ve el elemento con el mayor
valor de esa propiedad  

```css
h1 {
  position: fixed;
  z-index: 10;}
```

### Tipos de Layout

* **Anchura fija**

La anchura se especifica en pixeles (a veces tambien la altura)

![css9](/_img/html-css/fixedWidthLayout.png)

```css
body {
  width: 960px;
  margin: 0 auto;}
#content {
  overflow: auto;
  height: 100%;}
#nav, #feature, #footer {
  background-color: #efefef;
  padding: 10px;
  margin: 10px;}
.column1, .column2, .column3 {
  background-color: #efefef;
  width: 300px;
  float: left;
  margin: 10px;}
li {
  display: inline;
  padding: 5px;}
```

* **Liquidos**

La anchura se especifica en porcentajes.

* `<body>` 90% de anchura
* cada columna se le pone un `margin: 1%`
* `min-width, max-width` para limitar los estiramientos

![css10](/_img/html-css/liquidLayout.png)

```css
body {
  width: 90%;
  margin: 0 auto;}
#content {overflow: auto;}
#nav, #feature, #footer {
  margin: 1%;}
.column1, .column2, .column3 {
  width: 31.3%;
  float: left;
  margin: 1%;}
.column3 {margin-right: 0%;}
li {
  display: inline;
  padding: 0.5em;}
#nav, #footer {
  background-color: #efefef;
  padding: 0.5em 0;}
#feature, .article {
  height: 10em;
  margin-bottom: 1em;
  background-color: #efefef;}
```

* **Diseño 960px**

![css11](/_img/html-css/960pixelWide.png)

---

## IMAGENES

* **Controlar tamaño**

Creas tres clases de imagenes, pequeñas, medianas y grandes. Despues a cada
imagen le pones la clase a la que pertenece y asi automaticamente se pone a ese
tamaño

```css
img.large {
  width: 500px;
  height: 500px;}
img.medium {
  width: 250px;
  height: 250px;}
img.small {
  width: 100px;
  height: 100px;}
```

* **Alinear imagenes**

Se recomienda usar `float` en lugar de `align`. Creamos clases `align-left`
`align-right` y luego a las imagenes les añadimos la clase que nos interese

```css
img.align-left {
  float: left;
  margin-right: 10px;}
img.align-right	{
  float: right;
  margin-left: 10px;}
```

* **Centrar imagenes**

Debemos convertir la imagen en en elemento de bloque y luego usar `margin`

```css
img.align-center {
  display: block;
  margin: 0px auto;}
```

* **background**

`background` - Orden de las propiedades:

1. background-color
2. background-image
3. background-repeat
4. background-attachment
5. background-position

```css
body {
  background: #ffffff url("images/tulip.gif") no-repeat top right;}
```

`background-image` - la imagen se ponde de fondo del elemento al que se lo asignamos

```css
p	{
  background-image: url("images/pattern.gif");}
```

`background-repeat`- 

* `repeat` la imagen se repite horizontal y verticalmente
* `repeat-x` la imagen solo se repite horizontalmente
* `repeat-y` la imagen solo se repite verticalmente
* `no-repeat` la imagen se muestra solo una vez

```css
body {
  background-image: url("images/header.gif");
  background-repeat: repeat-x;}
```

`background-attachment`- 

* `fixed` la imagen permanece fija
* `scroll` la imagen se mueve con el scroll

```css
body {
  background-image: url("images/tulip.gif");
  background-repeat: no-repeat;
  background-attachment: fixed;}
```

`background-position`- Para especificar donde sale una imagen que no se repite.
La propiedad tiene dos valores: Si solo se indica uno el segundo valor sera `center`. Tambien se puede usar px o porcentajes   

1. posicion horizontal `left|center|right`
2. posicion vertical `top|center|bottom`

```css
body {
  background-image: url("images/tulip.gif");
  background-repeat: no-repeat;
  background-position: center top;}
body {
  background-image: url("images/tulip.gif");
  background-repeat: no-repeat;
  background-position: 50% 50%;}
```

### rollover y sprites

Enlaces o botones que tienen un segundo estilo al ponerse el puntero encima y un
tercer estilo al pincharlos.

Se usan `sprites` que son una sola imagen con diferentes partes a usar.

![css12](/_img/html-css/sprite.png)

```html
<a class="button" id="add-to-basket">Add to basket</a>
<a class="button" id="framing-options">Framing options</a>
```

```css
a.button {
  height: 36px;
  background-image: url("images/sprite.png");
  text-indent: -9999px;
  display: inline-block;}
a#add-to-basket {
  width: 174px;
  background-position: 0px 0px;}
a#framing-options {
  width: 210px;
  background-position: -175px 0px;}
a#add-to-basket:hover {
  background-position: 0px -40px;}
a#framing-options:hover {
  background-position: -175px -40px;}
a#add-to-basket:active {
  background-position: 0px -80px;}
a#framing-options:active {
  background-position: -175px -80px;}
```

---

## TRANSITION

### Ejemplo

```css3
.tecla {
  color: deepskyblue;
  background: rgba(0, 0, 0, 0.4);
  text-shadow: 0 0 1px black;
  transition: background-color 0.2s;
}
.pulsada {
  background-color: orange;
  transform: scale(1.1, 1.1);
}
```

```html
<div class="tecla" id="0">A<span class="sonido">clap</span></div>
```

```javascript
function pressKey (e) {
  var valor = mapa.indexOf(e.key.toUpperCase());
  if (valor !== -1) {
    document.getElementById(valor).addEventListener('transitionend',
      function () {
        document.getElementById(valor).classList.remove('pulsada');
      });
    document.getElementById(valor).classList.add('pulsada');
    audio[valor].play();
  }
}
```

---

## FLEXBOX

[FlexBox Guia](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

[Patrones de diseño con Flexbox](http://www.flexboxpatterns.com/home)

[Cheatsheet](https://www.alsacreations.com/xmedia/tools/flexbox-cheatsheet.pdf)

