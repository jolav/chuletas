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

* el objeto superior es `document` que representa la pagina como un todo

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

---

## EVENTOS

![js2](/z-static/images/comingSoon2.jpg)

---

## AJAX

![js2](/z-static/images/comingSoon2.jpg)

---


```js

```
