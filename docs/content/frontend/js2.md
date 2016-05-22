# JAVASCRIPT 2

---

## BUILT-IN OBJECTS

### Document Object Model

[Document Object Model](#dom)

### Browser Object Model

* El objeto superior es `window` que representa la ventana o pestaña del 
navegador.

![js2](/z-static/images/js/bom.png)

* Propiedades

`window.innerHeight` - altura (excluding browser chrome/user interface) (px)   
`window.innerWidth` - anchura (excluding browser chrome/user interface) (px)  
`window.pageXOffset` - Distance document has been scrolled horizontally (px)  
`window.pageYOffset` - Distance document has been scrolled vertically (px)  
`window.screenX` - Coordenada X, respecto esquina superior-izquierda (px)  
`window.screenY` - Coordenada Y, respecto esquina superior-izquierda (px)
`window.location` - URL actual (o ruta local al archivo)  
`window.document` - Referencia al objeto documento, usado para representar la 
pagina actual contenida en windows  
`window.history` - Referencia al objeto history de la pestaña o ventana del navegador que contiene detalles de las paginas que han sido vistas en esa 
ventana o pestaña  
`window.history.length` - numero de historias en el objeto history para esa 
pestaña o ventana del navegador  
`window.screen` - Hace referencia al objeto screen del monitor  
`window.screen.width` - Anchura de la pantalla del monitor(px)  
`window.screen.height` - Altura de la pantalla del monitor (px)  

* Metodos

`window.a1ert()` - Crea una caja de dialogo con mensaje. El usuario debe 
pinchar OK para cerrarla    
`window.open()` - Abre una nueva ventana de navegador cuya URL es la 
especificada como parametro (no funciona si el navegador tiene bloqueado los 
pop-up)    
`window.print()` - Avisa al navegador que el usuario quiere escribir contenidos 
de la pagina actual (como si el usuario hubiera pulsado la opcion de imprimir 
del navegador)   

### Global Javascript Objects

* Es un grupo de objetos individuales que cada uno hace referencia a una parte
del lenguaje javascript

![js2](/z-static/images/js/gjo.png)

#### [String](/frontend/js1/#string)

#### [Numero](/frontend/js1/#numeros)

#### [Math](/frontend/js1/#math)

#### Date

> * Creamos un objeto Date sobre el que luego aplicaremos los metodos
```js
var fecha = new Date();
```

* Metodos

`getDate()` , `setDate()` - Dia del mes (1-31)  
`getDay()` - Dia de la semana (0-6)  
`getFullYear()` , `setFullYear()` - año (4 digitos)  
`getHours()` , `setHours()` - Hora (0-23)  
`getMilliseconds()` , `setMilliseconds()` - Milisegundos (0-999)   
`getMinutes()` , `setMinutes()` - Minutos (0-59)  
`getMonth()` , `setMonth()` - Mes (0-11)  
`getSeconds()` , `setSeconds()` - Segundos (0-59)  
`getTime()` , `setTime()` - Milisegundos desde 1 Enero 1970 00:00:00 y negativo 
para cualquier tiempo anterior a esa fecha  
`getTimezoneOffset()` - Minutos de diferencia horaria entre UTC y la hora local   
`toDateString()` - Fecha del tipo `Mon Jan 17 1982`   
`toTimeString()` - Hora del tipo `19:45:15 GMT+0300 (CEST)`  
`toString()` - Devuelve una string con la fecha especificada  

---

## DOM

> Hay 3 tecnicas de actualizar contenido HTML  
1. `document.write()`  
2. *`element`*`.innerHTML`   
3. Manipulacion del DOM

### Arbol DOM

> El arbol DOM es una maqueta de una pagina web

![js2](/z-static/images/js/domTree.png)
  
* ![js2](/z-static/images/js/docNodes.png)  
En la parte superior del arbol esta un nodo documento que representa la
pagina entera. Es el punto de arranque para todas las rutas por el arbol DOM.  
Coincide con el `document object`  

* ![js2](/z-static/images/js/elementNodes.png)  
Los nodos elemento es lo que se busca en el arbol DOM antes de acceder a los 
nodes de atributo y texto  
Para eso estan los metodos que permiten buscar y acceder a nodos elemento

* ![js2](/z-static/images/js/attributeNodes.png)  
Las etiquetas abiertas de los elementos HTML pueden contener atributos que 
estan representados por nodos atributos en al arbol DOM  
No son hijos del elemento que los tiene, son parte de ese elemento, por eso
hay metodos y propiedades especificas para leer y modificar esos atributos  

* ![js2](/z-static/images/js/textNodes.png)  
Una vez accedido al nodo elemento puedes coger el texto dentro del elemento
que esta contenido en su propio nodo texto  
Pueden tener hijos pero seran hijos del elemento que los contiene  

### Objeto document

> El objeto superior es `document` que representa la pagina como un todo

![js2](/z-static/images/js/dom.png)

* Propiedades

`document.title` - Titulo del documento actual  
`document.lastModified` -  Fecha de la ultima modificacion del documento actual  
`document.URL` - Devuelve una cadena con la URL del documento actual  
`document.domain` - Devuelve el dominio del documento actual  

* Metodos

`document.write()` - Escribe texto al documento  
`document.getElementByld()` - Devuelve el elemento que coincide con el valor de 
id que se pasa  
`document.querySe1ectorA11()` - Devuelve lista de elementos que coincide con 
el selector CSS que se pasa como parametro  
`document.createElement()` - Crea un nuevo elemento  
`document.createTextNode()` - Crea un nuevo nodo de texto  

### Acceder a elementos

* Seleccionar un elemento

`document.getElementById("name")` - Selecciona el elemento con id="name"   
`document.querySelector("h4")` - Selecciona el primer elemento h4  
*`element`*`.parentNode` - Toma el padre del elemento actual  
*`element`*`.previousSibling` - Coge el hermano previo  
*`element`*`.nextSibling` - Coge el hermano siguiente  
*`element`*`.firstChild` - Coge el primer hijo del actual elemento  
*`element`*`.lastChild` - Coge el ultimo hijo del actual elemento  

```
¡ OJO ! En estos 5 ultimos con los WhiteSpace Nodes
```

* Seleccionar varios elementos : Los metodos devuelven una `NodeList`

`document.getElementsByClassName("name")` - Selecciona todos los elementos 
que tienen la clase="name"   
`document.getElementsByTagName("li")` - Coge todos los elementos que tienen 
el tag name="li"   
`document.querySelectorAll(h4)` - Selecciona todos los elementos que poseen h4   

### Trabajar con elementos

#### Propiedades  

*`element`*`.nodeValue` - Permite acceder al contenido de un nodo texto   
*`element`*`.innerHTML` - Permite acceder a elementos hijo y contenido de texto
y de markup    
*`element`*`.textContent` - Permite acceder al texto del elemento y de sus 
hijos 
*`element`*`.className` - Valor del atributo class  
*`element`*`.id` - Valor del atributo id  

![js2](/z-static/images/js/accessText.png)

* *`element`*`.nodeValue`  
Una vez llegas de un elemento a su nodo texto, este tiene la propiedad 
nodeValue que te da acceso al valor del texto  

```js
document.getElementByid("one").firstChild.nextSibling.nodeValue;
```

* *`element`*`.textContent`  
Permite acceder al texto del elemento y de sus hijos 

```js
document.getElementByid("one").textContent;
```

* *`element`*`.innerText` **NO USAR**  

* *`element`*`.innerHTML`

```js
var elContent = document.getElementByld('one').innerHTML;     // Get
// elContent =  "<em>fresh</em> figs"
document.getElementByld('one').innerHTML = elContent;         // Set
```

#### Manipulacion DOM

`document.createElement()` - Crea nodos  
`document.createTextNode()` - Crea nodos de texto  
*`element`*`.appendChild()` - Añade nodos  
*`element`*`.removeChild()` - Elimina nodo    
*`element`*`.hasAttribute(n)` - Chequea si existe el atributo n  
*`element`*`.getAttribute(n)` - Consigue el valor del atributo n  
*`element`*`.setAttribute(n)` - Pone el valor del atributo n  
*`element`*`.removeAttribute(n)` - Elimina el atributo n 

> Crear nuevo contenido en la pagina

1. `document.createElement()` - Crea un nuevo nodo elemento y se guarda en 
una variable  
2. `document.createTextNode()` - Crea un nuevo nodo texto y se guarda en una 
variable  
3. *`element`*`.appendChild()` - Lo añade al arbol DOM como un hijo

```js
var newEl = document.createElement("li");
var newText document.createTextNode("quinoa");
newEl.appendChild(newText);
// Ahora lo posicionamos donde le corresponda
var position = document.getElementsByTagName("ul")[O];
position.appendChild(newEl);
```

> Eliminar contenido en la pagina

1. Almacenar el elemento a borrar en una variable  
2. Almacenar el padre del elemento a borrar en una variable  
3. Eliminar el elemento desde su elemento padre  

```js
var removeEl = document.getElementsByTagName("li")[3];
var containerEl = removeEl.parentNode;
containerEl.removeChild(removeEl);
```

### NodeLists

> Son `collections`, parecen `arrays` y estan numerados como tal pero no lo son

* Propiedades

`length` - indica cuantos items hay en la NodeList  

* Metodos

`item(n)` - devuelve el item con indice numero n  
`nodeLista[n]` - es mas comun esta sintaxis con corchetes  

```js
var lista = document.getElementsByClassName("hot")
if (lista.length >= 1) {
  var primero = lista.item(0);
  var primero = lista[0];
}
```

* Iteraciones

```js
var lista = document.querySelectorAll("li.cold")
for (var i = 0; i < lista.lenth; i++) {
  lista[i].className = "hot";
}
```

### XSS Atacks

> **Escapar todo el contenido que generan los usuarios**  
Todos los datos introducidos por usuarios deben escaparse en el servidor 
antes de ser mostrados en la pagina.    

* HTML - Escapar todos estos caracteres para que se muestren como caracteres y
no sean procesados como codigo  

      ![js2](/z-static/images/js/escapeHTML.png)

* Javascript -No incluir datos de fuentes sin confianza.Escapar todos los 
caracteres ASCII de valor menor a 256 que no sean caracteres alfanumericos  

* URLs - Si tienes enlaces que datos que han introducido los usuarios usa el
metodo `encodeURIComponent()` para codificar los siguientes caracteres :

      ![js2](/z-static/images/js/escapeURLs.png)

> **Añadir contenido del usuario**  

* Javascript:
    - Usar `textContent` o `innerText`  
    - NO usar `innerHTML`  

* jQuery:
    - Usar `.text()`  
    - NO Usar `.html()` salvo:
        - No permitir al usuario contenido con markup
        - Escapar todo el contenido del usuario y añadirlo como texto en lugar
          de como HTML

---

## EVENTOS

> 1. Las Interacciones del usuario crean Eventos
> 2. Los Eventos disparan Codigo
> 3. El Codigo responde a los usuarios (p ej : modificando el DOM)

1. Seleccionar el elemento que queremos tenga un evento  
2. Indicar que evento en ese elemento disparara la respuesta  
3. Indicar el codigo a ejecutar cuando ocurra el evento 

### Lista Eventos

* User Interface Events

`load` - La pagina web se ha terminado de cargar  

```js
function setup() {         
  // Al haber cargado la pagina ya esta el contenido disponible                                                          
}
window.addEventListener('load', setup, false);
```

`unload` - La pagina web se esta descargando a causa de haber pedido otra  
`beforeunload` - Justo antes de que el usuario abandone la pagina   
`error` - Navegador encuentra un error de Javascript  
`resize` - Navegador se ha redimensionado  
`scroll` - Usuario ha hecho scroll hacia arriba o abajo, puede referirse a la
pagina entera o a un elemento especifico como por ejemplo textarea  

* Keyboard Events  

`keydown` - Pulsar una tecla(se repite mientras la tecla esta presionada)  
`keyup` - Soltar una tecla  
`keypress` - Mientras se inserta el caracter (se repite mientras la tecla esta presionada)  

* Mouse Events 

`click` - Pulsar y soltar un boton sobre el mismo elemento  
`dblclick` - Pulsar y soltar un boton dos veces sobre el mismo elemento  
`mousedown` - Pulsar un botón del ratón mientras esta sobre un elemento  
`mouseup` - Soltar un botón del ratón mientras esta sobre un elemento  
`mousemove` - Mover el raton   
`mouseover` - Mover el raton por encima de un elemento  
`mouseout` - Mover el raton fuera de un elemento  

* Focus Events

`focus` , `focusin` - EL elemento gana el foco  
`blur` , `focusout` -  EL elemento pierde el foco  
 
* Form Events

`input` - Cambia el valor de cualquier `<input>`, `<textarea>` o elemento con 
el atributo `contenteditable`  
`change` - Cambia el valor en un `select box`, `checkbox` o `radio button`  
`submit` - Al enviar un formulario (usando un boton o una tecla)  
`reset` - Usuario pincha un boton de resetear formulario (Muy raro verlo)  
`cut` -  Usuario corta contenido de un campo del formulario  
`copy` - Usuario copia contenido de un campo del formulario  
`paste` - Usuario pega contenido a un campo del formulario  
`select` - Usuario selecciona algo de texto en un campo de formulario  

* Mutation Events : Ocurren cuando la estructura DOM cambia por un script 
 
`DOMSubtreeModified` - Cambio affecta al documento  
`DOMNodeInserted` - Se ha insertado un nodo como hijo de otro nodo  
`DOMNodeRemoved` - Se ha eliminado un nodo desde otro nodo  
`DOMNodeInsertedIntoDocument` - Se ha insertado un nodo como descendiente de 
otro nodo  
`DOMNodeRemovedFromDocument` - Se ha eliminado un nodo como descendiente de
otro nodo  

* HTML5 Events

`DOMContentLoaded` - Salta cuando el arbol DOM (toda la estructura HTML) ya 
esta cargada) a diferencia de `load` que salta cuando todo el HTML y sus 
recursos (imagenes, iframes, scripts, css) se han cargado.  
Asi la pagina parece que es mas rapida de cargar. Se asigna al `document` o 
al `window`    
`hashchange` - Salta cuando hay cambios en la URL a partir del ancla #.  
Funciona sobre el objeto `window` y despues de saltar el objeto `event` tendra
dos propiedades `oldURL`, `newURL`     
`beforeunload` - Salta en el objeto `windows` antes de tirar la pagina. Es el 
que usan para molestar diciendo si estas seguro de querer abandonar la pagina  

### Manejadores Eventos 

* HTML Event Handlers `NO USAR`  

```html
<form method="post" action="http://www.example.org/register">
  <label for="username">Create a username: </label>
  <input type="text" id="username" onblur="checkUsername()" />
  <div id="feedback"></div>
  <label for="password">Create a password: </label>
  <input type="password" id="password" />
  <input type="submit" value="sign up!" />
</form>
```

```js
function checkUsername() {                             
  var elMsg = document.getElementById('feedback');     
  var elUsername = document.getElementById('username');
  if (elUsername.value.length < 5) {                   
    elMsg.textContent = 'Username must be 5 characters or more'; 
  } else {                                               
    elMsg.textContent = '';                             
  }
}
```

* Traditional DOM Event Handlers  
La pega es que solo puedes vincular una funcion a un evento  
*`element`**`.on`**`event`*` = `*`functionName`*; 

```js
function checkUsername() {                             
  // codigo que sea
}
var el = document.getElementByld("username") ;
el.onblur = checkUsername;
```

* Event Listeners (DOM level 2 ) `USAR`  
Permite que un solo evento dispare varios scripts

### Event Listener

* **Concepto**

![js2](/z-static/images/js/eventListener.png)

* **Ejemplo**

```js
function checkUsername() {                             
  // codigo que sea
}
var el = document.getElementByld("username") ;
el.addEventListener("blur", checkUsername, false);
```

* **Usando Parametros**  

![js2](/z-static/images/js/parametersEvents.png)

```js
var elUsername = document.getElementById('username');   
var elMsg      = document.getElementById('feedback');   

function checkUsername(minLength) {                     
  if (elUsername.value.length < minLength) {            
    elMsg.innerHTML = 'Username must be ' + minLength + ' chars or more';
  } else {                                             
    elMsg.innerHTML = '';                              
  }
}

elUsername.addEventListener('blur', function() {        
  checkUsername(5);                                     
}, false);
```

### Event FLow

> Describe el orden en el que los eventos se procesan

* `bubbling` - el evento se captura y maneja primero por el elemento mas interno y 
despues de propaga hacua los elementos exteriores

* `Capturing` - el evento se captura primero por el elemento mas externo y se 
propaga hacia los elementos internos

![js2](/z-static/images/js/eventFlow.png)

### Objeto Event

* Propiedades
  
`target` - Objetivo del evento  
`type` - Tipo de evento que se ha disparado  
`cancelable` - Si se puede cancelar el comportamiento predeterminado de un elemento  
`screenX` , `screenY` - Coordenada de la posicion del cursor respecto del
monitor  
`pageX` , `pageY` - Coordenada de la posicion del cursor respecto de la 
pagina  
`clientX` , `clientY` - Coordenada de la posicion del cursor respecto del 
navegador  

* Metodos  

`preventDefault()` - Cancela el comportamiento por defecto del evento (si es
posible)  
`stopPropagation()` - Detiene la propagacion del evento por bubbling o 
capturing  

* Event listener sin parametros

La referencia al objeto event se pasa automaticamente aunque en la funcion debe
nombrarse (lo normal es `e` para `event`)  

```js

function checkUsername(e) {
  var target = e.target;
}
var el = document.getElementById("username");
el.addEventListener("blur", checkUsername, false);
```

* Event listener con parametros

La referencia al objeto event se pasa automaticamente pero debe nombrarse en 
los parametros  

```js
function checkUsername(e, min) {
  var target = e.target;
}
var el = document.getElementById("username");
el.addEventListener("blur", function(e) {
  checkUsername(e, 5);
}, false);
```

### Delegacion

Para evitar malos rendimientos por ejemplo en listas o celdas o tablas se pone
el `event listener` en el elemento que los contiene y luego se usa la propiedad
`target` del objeto `event` para encontrar al hijo que desato el evento

---

## AJAX

![js2](/z-static/images/comingSoon2.jpg)

---


```js

```
