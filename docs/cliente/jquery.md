# JQUERY

---

## INSTALACION

```html
// jQuery CDN
<script src="https://code.jquery.com/jquery-2.2.4.min.js"</script>
<script src="https://code.jquery.com/jquery-1.12.4.min.js"</script>
// Google CDN
<script
src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.3/jquery.min.js">
</script>
<script
src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.3/jquery.min.js">
</script>
// Microsoft CDN
<script
src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js">
</script>
<script
src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js">
</script>
```

```html
// Seguro por si falla el CDN usar la copia local del servidor
<script>
window.jQuery ||
document.write("<script src="js/jquery-1.12.4.js"></script>")
</script>
```

Todos los elementos `<script></script>` de la pagina van mejor al final de la
misma justo antes de la etiqueta `</body>` para no afectar el renderizado del
resto de la pagina  

[Documentacion jQuery](http://api.jquery.com)

## SELECTORES

* **Basicos**

`*` - Todos los elementos  
`element` - Todos los elementos con ese nombre  
`#id` - Elementos con ese id  
`.class` - Elementos con esa clase  
`selector1, selector2` - Elementos que coinciden con mas de un selector  

* **Jerarquia**  

`ancestor descendant` - Elemento que es descendiente de otro elemento  
`parent > child` - Elemento hijo directo de otro elemento  (si pones * en
child seleccionas todos los hijos del elemento especificado)  
`previous + next` -  Elementos que son inmediatamente seguidos por el previo  
`previous - siblings` - Elementos que son hermanos del previo  

* **Filtros Basicos**  

`:not(selector)` - Todos excepto el indicado  
`:first` - El primer elemento de la seleccion  
`:last` -  El ultimo elemento de la seleccion  
`:even` - Elementos pares de la seleccion  
`:odd` - Elementos impares de la seleccion  
`:eq(index)` - Elementos con un indice igual al del parametro  
`:gt(index)` - Elementos con un indice mayor al del parametro  
`:lt(index)` - Elementos con un indice menor al del parametro  
`:header` - Todos los elementos `<h1>...<h6>`  
`:animated` - Elementos actualmente animados  
`:focus` - Elemento que tiene el foco  

```js
$(document.activeELement)         // Da el mejor rendimiento
```  

* **Filtros de contenido**  

`:contains("text")` - Elementos con el texto especificado como parametro  
`:empty` -  Todos los elementos que no no tienen hijos  
`:parent` - Todos los elementos que tienen un hijo  
`:has(selector)` - ELementos que al menos tienen un elemento que coincide con
el selector  

* **Filtros de Visibilidad**  

`:hidden` - Todos los elementos ocultos  
`:visible` - Todos los elementos que consumene espacio en el diseño de la
pagina  

* **Filtros de hijos**    

`:nth-child(expre)` - Valor que no sea cero  
`:first-child` - Primer hijo de la seleccion actual  
`:last-child` - Ultimo hijo de la seleccion actual  
`:only-child` - Cuando solo hay hijo del elemento  

* **Filtros de atributos**  

`[attribute]` - Elementos con el atributo especificado y da igual el valor  
`[attribute = "value"]` - Elementos con el atributo y el valor especificado   
`[attribute != "value"]` - Elementos con el atributo especificado pero sin el
valor especificado  
`[attribute ^= "value"]` - El valor del atributo empieza con este valor   
`[attribute = "value"]` - El valor del atributo termina con este valor  
`[attribute* = "value"]` - El valor aparece en cualquier lugar del valor  
`[attribute| = "value"]` - Igual a una cadena dada o empezando con cadena y
seguir con un guion  
`[attribute~ = "value"]` - El valor es uno de los valores de la una lista
separada con espacios  
`[attribute][attribute2]` - Elementos que cumplen todos los selectores  

* **Formularios**  

`:input` -

> Todos los elementos de entrada de texto `<button>`, `<input>`, `<select>`,
> `<textarea>`  

```js
.filter(":input");                // Da el mejor rendimiento
```


`:text` -

> Selecciona todas los elementos `<input>` con un atributo `type`
> cuyo value es `text` o que no tiene atributo `type`   

```js
('input:text');                   // Da el mejor rendimiento
```

`:password` - Todos las entradas de passwords  

```js
$("input:password");              // Da el mejor rendimiento
```

`:radio` - Todos los radio botones  

```js
$('input(name="gender"]:radio')   // Da el mejor rendimiento
```

`:checkbox` -

> ELementos `<input>` cuyos atributos `type` tengan un valor de `checkbox`  

```js
$('[type="checkbox"]');           // Da el mejor rendimiento
```

`:submit` -

> Todos los elementos `<button>` y `<input>` cuyos atributos `type` tienen un
> value de `submit`

```js
$('[type="submit"]');              // Da el mejor rendimiento
```

`:image` - Todos los elementos que son entradas de imagenes

```js
$('[type="image"]');              // Da el mejor rendimiento
```

`:reset` - Todos los botones de reseteo  
`:button` -

> Todos los elementos `<button>` y `<input>` que tengan un atributo
> `type` con valor de `button`

`:file` -  Todos los elementos que son entradas de archivos  
`:selected` -  Todos los elementos que estan seleccionados  

```js
.filter(":selected")              // Da el mejor rendimiento
```

`:enabled` - Todos los elementos de formularios activos    
`:disabled` - Todos los elementos de formularios inactivos  
`:checked` - Elementos `checked` de checkboxes y radio botones   

---

## METODOS

 * **Filtros de contenido**  

*GET - SET CONTENIDO*

`.html(string o funcion)` -  

> Get, solo da la informacion del primer elemento de la seleccion y la
> de todos sus descendientes

`.text(string o funcion)` -

> Get, da la informacion de todo los elementos de la seleccion y la de
> todos sus descendientes


`.replaceWith(string o funcion)` -  
`.remove()` -

> Para los SET los 4 afectan a todo el contenido de la seleccion sobre la que
> se aplican  

*ELEMENTOS*  

`.before()` -  
`.after()` -  
`.prepend()` -  
`.append()` -  

> ![](/_img/javascript/insertElements.png)

`.remove()` - Elimina los elementos del arbol DOM  
`.clone()` - Crea una copia de los elementos coincidentes incluyendo
descendientes y nodos de texto   
`.unwrap()` -  Elimina los padres de los elementos coincidentes pero respeta
a estos  
`.detach()` - Igual que `.remove()` pero mantiene una copia en memoria  
`.empty()` - Elimina los nodos hijo y descendientes de los elementos
coincidentes  
`.add()` - Selecciona todos los elementos que contienen el texto especificado  

*ATRIBUTOS*

`.attr()` -  

```js
$('li#one').attr('id');            // GET
$('li#one').attr('id', 'hot');     // SET
```

`.removeAttr()` -  
`.addClass()` -  
`.removeClass()` -  
`.css()`  

```js
var backgroundColor = $('li').css('background-color');   // GET  
${'li').css('padding-left', '+=20');                     // SET
$('1i').css({                                            // SET multiple
  'background-color': '#272727',
  'font-family': 'Courier'
)};
```

*VALORES DE FORMULARIO*  

`.val()` -  

> Devuelve el valor del primer elemento `<input>`, `<select>` y `<textarea>`
> de la seleccion.  
> Tambien se puede usar para determinar el valor

`.isNumeric(valor)` -

> Chequea si el valor es un valor numerico y devuelve `true` si es asi  

```js
// Todos estos devuelven true
$.isNumeric(l)            $.isNumeric(-3)         $.isNumeric("2")
$.isNumeric(4.4)          $.isNumeric(+2)         $.isNumeric{OxFF)
```

* **Busqueda de elementos**  

*GENERAL*  

`.find(selector)` - Pilla todos los elementos de la actual seleccion que
cumplen con el selector  
`.closest(selector)` - Ancestro mas cercano que cumple con el selector      
`.parent()` - Padre directo de la actual seleccion   
`.parents()` - Todos los padres de la actual seleccion  
`.children()` - Todos los hijos de la actual seleccion  
`.siblings()` - Todos los hermanos de la actual seleccion   
`.next()` - Siguiente hermano del elemento actual  
`.nextAll()` - Todos los subsiguientes hermanos del elemento actual  
`.prev()` - Hermano precio al elemento actual  
`.prevAll()` - Todos los hermanos previos al elemento actual  

*FILTROS Y TEST*  

`.filter(selector)` - Encuentra elementos en la seleccion que cumplen con este
segundo selector  
`.not() | :not(selector)` - Encuentra elementos que no cumplen con el selector  
`.has() | :has(selector)` - Encuentra descendientes de los elementos en la seleccion que cumplen con este segundo selector   

```js
$('li').not('.hot').addClass('cool');       // Estos selectores
$('li:not(.hot)').addClass('cool');         // son equivalentes
```

`.is(condicion)` - Devuelve booleano de chequear si la actual seleccion cumple la
condicion    
`:contains('texto')` - Selecciona todos los elementos que contienen el texto
especificado (distingue may/min)   

*ORDEN DE LA SELECCION*    

`.eq(indice)` - El elemento que coincide con el indice    
`.lt(indice)` - Elementos con indice menor que el especificado   
`.gt(indice)` - Elementos con indice mayor que es especificado  

```js
$(function() {
  $('li:lt(2)').removeClass('hot');
  $('li').eq(O).addClass('complete');
  $('li:gt(2)').addClass('cool');
});
```

* **Dimensionamiento y posicion**  

*DIMENSION*    

`.height()` - Altura de la caja  
`.width()` - Anchura de la caja  
`.innerHeight()` - Altura + padding  
`.innerWidth()` - Anchura + padding  
`.outerHeight()` - Altura + padding + border  
`.outerWidth()` - Anchura + padding + border  
`.outerHeight(true)` - Altura + padding + border + margin  
`.outerWidth(true)` - Anchura + padding + border + margin  

> ![jquery1](/_img/javascript/boxDimensions.png)

`$(document).height()` -  
`$(document).width()` -  
`$(window).height()` -  
`$(window).width()` -  


*POSICION*  

`.offset()` - Devuelve o establece las coordenadas del elemento relativas a la
esquina superior izquierda del `documento object`  
`.position()` - Devuelve o establece las coordenadas del elemento relativas a
cualquier ancestro que haya sido sacado de su flojo normal. Si no hay ancestro
fuera del flujo normal pues nos dara lo mismo que `.offset()`  
`.scrollLeft()` - Devuelve o establece la posicion horizontal de la barra de
scroll   
`.scrollTop()` - Devuelve o establece la posicion vertical de la barra de scroll  

* **Efectos y animacion**  

*BASICO*    

`.show()` - Muestra los elementos seleccionados   
`.hide()` - Oculta los elementos seleccionados   
`.toggle()` -  Alterna entre mostrar y ocultar los elementos seleccionados  

*FADING*  

`.fadeIn()` - Funde los elementos seleccionados haciendolos opacos  
`.fadeOut()` - Desdibuja los elementos seleccionados haciendolos transparentes  
`.fadeTo()` -  Cambia la opacidad de los elementos seleccionados  
`.fadeToggle()` - Muestra u oculta los elementos cambiando su opacidad al nivel
contrario del que este  

*SLIDING*    

`.slideUp()` - Muestra los elementos seleccionados deslizandolos hacia arriba    
`.slideDown()` - Oculta los elementos seleccionados deslizandolos hacia abajo  
`.slideToggle()` - Muestra u oculta los elementos seleccionados deslizandolos
en la direccion contraria a su actual estado  

*CUSTOM*    

`.delay()` - Retrasa la ejecucion del codigo   
`.stop()` - Detiene la animacion si esta corriendo  
`.animate()` - Crea animaciones de diseño  

> ![jquery1](/_img/javascript/animatingCSS.png)

> 1. `Speed`: duracion en milisegundos de la animacion  
> 2. `Easing`  
>     * `linear`: la velocidad de la animacion es constante  
>     * `swing`: Se acelera en el medio y es mas lenta al comienzo y final  
> 3. `Complete`: funcion `callback` que se ejecuta al terminar la animacion


> Equivalencias en jQuery de propiedades CSS

> ![jquery1](/_img/javascript/propertyNames.png)

```js
$(function() {
  $('li').on('click', function() {
    $(this).animate({
      opacity: 0.0,
      paddingLeft: '+=80'
    }, 500, function() {
      $(this).remove();
    });
  });
});
```



* **Eventos**  

*ARCHIVOS - DOCUMENTOS*    

`.ready()` -  
`.load()` -  

*USUARIO*    

`.on()` -  [Eventos](#eventos)  

---

## CONCEPTOS

### Get/Set simples/multiples

> Cuando seleccionas uno o mas elementos se devuelve un `objeto jQuery`
> conocido como `matched set` o `jquery selection`  

* Un solo elemento, cada elemento tiene un indice en esta caso solo uno, el
indice cero  

* Multiples elementos, el objeto jquery tiene varios elementos cada uno con
un indice diferente desde 0 hasta n-1   
    * Get : Si usas un metodo para conseguir la informacion del elemento te
    dara la informacion del primer elemento  
    * Set : Si usas un metodo para actualizar la informacion se actualizara
    todos los elementos reultantes de la busqueda  

Para evitar estos comportamientos se pueden usar filtros o selecciones mas
especificas

### Bucles

`Iteracion implicita`  
Cuando un selecor devuelve multiples elementos se pueden actualizar todos
usando el mismo metodo  

```js
// Añadir la clase a todos los elementos em dentro de li
$('li em').addClass('seasonal') ;
```

`.each(funcion anonima o con mombre)`

* `this` es el elemento sobre el que el bucle esta en ese momento   
* `$(this)` crea una nueva `jquery seleccion` conteniendo el elemento actual  

```js
$('1i').each(function()
  var ids = this.id;
  $(this).append('<span class="order">' + ids + ' </span>') ;
});
```  

### Chaining

Aplicar varios metodos al mismo selector.   
SI se puede aplicar a los metodos que actualizan las selecciones jquery  
NO se puede aplicar a metodos devuelven informacion del DOM  

```js
// Lo oculta, espera 1/2 segundo y luego lo desvanece
$("li[id!="one"]").hide().delay(5OO).fadeln(1400);

// Asi mas claro
$("li[id! ="one"]")
  .hide()
  .delay(500)
  .fadeln(1400);
```

### Ready

```js
$(document).ready(function() {
  // codigo
)
// Abreviado
$(function() {
  // codigo
}
```

---

## EVENTOS

### .on() Metodo

Se usa para manejar todos los eventos, necesita dos parametros obligatorios mas
otros dos opcionales  

> ![jquery1](/_img/javascript/onParameters.png)

> 1. Obligatorio: Evento al que se a responder    
> 2. Opcional: Selector que filtra la seleccion inicial de elementos. Sera
> sobre ese subconjunto sobre el que se respondera a ese evento  
> 3. Opcional: Para pasar informacion extra a la funcion que se ejecutara
> cuando se dispare el evento  
> 4. Obligatorio: Codigo a ejecutar como respuesta al evento que es una
> funcion anonima o con nombre  
> 5. Automatico: Object event (e)

```js
$listltems.on('mouseout', function() {
  $(this).children('span').remove();
});
```

```js
$('ul').on(
  'click  mouseover',
  ':not(#four)',
  {status: 'important'},
  function(e) {
    listltem = 'Item: ' + e.target.textContent + '<br />';
    itemStatus = 'Status: ' + e.data.status + ' <br />';
    eventType = 'Event: ' + e.type;
    $('#notes').html(listItem + itemStatus + eventType);
  }
);
```

### Eventos jQuery

UI  
`focus`, `blur`, `change`  

KEYBOARD  
`input`, `keydown`, `keyup`, `keypress`  

MOUSE  
`click`, `dblclick`, `mouseup`, `mousedown`, `mouseover`, `mousemove`,
`mouseout`, `hover`  

FORM  
`submit`, `select`, `change`    

DOCUMENT   
`ready`, `load`, `unload`  

BROWSER  
`error`, `resize`, `scroll`  

### Objeto Event

Cada funcion que maneja un evento recibe un objeto `event`

```js
$('li').on('click', function(e) {
  $('li span').remove();
  var date = new Date();
  date.setTime(e.timeStamp);
  var clicked = date.toDateString();
  $(this).append(
    '<span class="date">' + clicked + ' ' + e.type + '</span>');
});
```

---

## AJAX

### Requests

* **Metodos**

`.load()` - Carga codigo HTML en el elemento seleccionado  
`$.get(url[, data][, callback][, type])` - Carga datos usando el metodo GET.
Se usa para pedir datos del servidor  
`$.post(url[, data][, callback][, type])` - Carga datos usando el metodo POST.
Se usa para actualizar datos
del servidor  
`$.getJSON(url[, data][, callback])` - Carga datos JSON usando el metodo GET.
Se usa para datos JSON   
`$.getScript(url[, callback])` - Carga y ejecuta datos javascript usando el
metodo GET. Se usa para datos javascript (p ej JSONP)  

> `url` de donde se sacan los datos  
> `data` ofrece informacion extra para enviar al servidor  
> `callback` la funcion que se ejecutara cuando recibamos los datos  
> `type` el tipo de dato que esperamos recibir del servidor  

`$.ajax()` - Se usa para hacer las peticiones. Todos los de arriba en realidad
usan este por debajo

> `type` - GET ó POST   
> `url` - Pagina a la que enviar la peticion  
> `data` - Datos adicionales que van al servidor con la peticion  
> `success` - Funcion que se ejecuta se la peticion tiene exito (similar a
> `.done()`)    
> `error` - Funcion que se ejecuta si la peticion falla (similar a `.fail()`)  
> `beforeSend` - Funcion que se ejecuta antes de que comienze la peticion (por
>  ejemplo para poner un icono de cargando)  
> `complete` - Funcion para ejecutar despues de los eventos de exito o fallo
> (por ejemplo para quitar el icono de cargando)  
> `timeout` - Milisegundos a esperar antes de que el evento falle  

```js
$('nav a').on('click', function(e) {
  e.preventDefault();
  var url = this.href;                                 // URL to load
  var $content = $('#content');                        // Cache selection
  $('nav a.current').removeClass('current');           // Update links
  $(this).addClass('current');
  $('#container').remove();                            // Remove content
  $.ajax({
    type: "GET",                                       // GET or POST
    url: url,                                          // Path to file
    timeout: 2000,                                     // Waiting time
    beforeSend: function() {                           // Before Ajax
      $content.append('<div id="load">Loading</div>'); // Load message
    },
    complete: function() {                             // Once finished
      $('#load').remove();                             // Clear message
    },
    success: function(data) {                          // Show content
      $content.html( $(data).find('#container') ).hide().fadeIn(400);
    },
    error: function() {                                // Show error msg
      $content.html('<div class="container">Please try again soon.</div>');
    }
  });
});
```

### Responses

* **Propiedades**

`responseText` - Datos basados en texto  
`responseXML` - Datos en XML  
`status` - Codigo de estado  
`statusText` - Descripcion del estado  

* **Metodos**

`.done()` - Codigo a ejecutar si la peticion tuvo exito  
`.fail()` - Codigo a ejecutar si la peticion fallo  
`.always()` - Codigo a ejecutar si la peticion tuvo exito o fallo  
`.abort()` - Cortar la comunicacion  

```js
function loadRates() {
  $.getJSON('data/rates.json')
  .done( function(data){                                 
    // codigo si la peticion tuvo exito                        
  }).fail( function() {                                  
    // codigo si la peticion ha fallado   
  }).always( function() {                                
    // codigo que se ejecuta siempre
  });
}
```

### Cargar HTML

> > ![jquery](/_img/javascript/loadingHTML.png)

> 1. Crea un objeto jQuery con el elemento cuya id="content"  
> 2. La URL de la pagina de donde queremos cargar el HTML. Debe haber un
> espacio entre la URL y el selector del paso 3  
> 3. Es el fragmento de la pagina HTML cargada que queremos mostrar. En este
> caso es la seccion con id="content"  

```html
<nav>
  <a href="jq-load.html" class="current">HOME</a>
  <a href="jq-load2.html">ROUTE</a>
  <a href="jq-load3.html">TOYS</a>
</nav>

<section id="content">
  <div id="container">    
    <!-- CONTENIDO DE LA PAGINA AQUI -->
  </div>
</section>
```

```js
$('nav a').on('click', function(e) {          // User clicks nav link
  e.preventDefault();                         // Stop loading new link
  var url = this.href;                        // Get value of href

  $('nav a.current').removeClass('current');  // Clear current indicator
  $(this).addClass('current');                // New current indicator

  $('#container').remove();                   // Remove old content
  // New content
  $('#content').load(url + ' #container').hide().fadeIn('slow');
});
```

### Enviando Formularios

* **Metodo**

`.serialize()`  
> * Selecciona toda la informacion del formulario  
> * La pone en un cadena lista para ser enviada al servidor  
> * Codifica los caracteres que no pueden ser enviados un cada cadena  
> * Solo envia controles de formulario exitosos, no envia controles
> desactivados ni no seleccionados ni el boton enviar  

```js
$('#register').on('submit', function(e) {           
  e.preventDefault();                               
  var details = $('#register').serialize();         
  $.post('register.php', details, function(data) {  
    $('#register').html(data);                    
  });
});
```



---


































```js

```
