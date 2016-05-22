# JQUERY

--- 

## Selectores

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
`:focus` - Elemento con el foco  

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

`:input` - Todos los elementos de entrada  
`:text` - Todos las entradas detextos    
`:password` - Todos las entradas de passwords  
`:radio` - Todos los radio botones  
`:checkbox` - Todos los checkboxes  
`:submit` - Todos los botones de envio  
`:image` - Todos los elementos de imagenes  
`:reset` - Todos los botones de reseteo  
`:button` - Todos los elementos botones  
`:file` -  Todas las entradas de archivos  
`:selected` - Todos los objetos seleccionados de listas drop-down  
`:enabled` - Todos los elementos de formularios activos    
`:disabled` - Todos los elementos de formularios inactivos  
`:checked` - Todos los checkboxes o radio botones chequeados  

---

# METODOS 


* **Filtros de contenido**  

CONSEGUIR - CAMBIAR CONTENIDO  
`.html()` -  
`.text()` -  
`.replaceWith()` -  
`.remove()` -  

ELEMENTOS  
`.before()` -  
`.after()` -  
`.prepend()` -  
`.append()` -  
`.remove()` -  
`.clone()` -  
`.unwrap()` -  
`.detach()` -  
`.empty()` -  
`.add()` -  

ATRIBUTOS  
`.attr()` -  
`.removeAttr()` -  
`.addClass()` -  
`.removeClass()` -  
`.css()` -  

VALORES DE FORMULARIO  
`.val()` -  
`.isNumeric()` -  

* **Busqueda de elementos**  

GENERAL  
`.find()` -  
`.closest()` -  
`.parent()` -  
`.parents()` -  
`.children()` -  
`.siblings()` -  
`.next()` -  
`.nextAll()` -  
`.prev()` -  
`.prevAll()` -  

FILTROS Y TEST  
`.filter()` -  
`.not()` -  
`.has()` -  
`.is()` -  
`:contains()` -  

ORDEN DE LA SELECCION  
`.eq()` -  
`.lt()` -  
`.gt()` -  

* **Dimensionamiento y posicion**  

DIMENSION  
`.height()` -  
`.width()` -  
`.innerHeight()` -  
`.innerWidth()` -  
`.outerHeight()` -  
`.outerWidth()` -  
`$(document).height()` -  
`$(document).width()` -  
`$(window).height()` -  
`$(window).width()` -  

POSICION  
`.offset()` -  
`.position()` -  
`.scrollLeft()` -  
`.scrollTop()` -  

* ** Efectos y animacion**  

BASICO  
`sahow()` -  
`.hide()` -  
`.toggle()` -  

FADING  
`.fadeIn()` -  
`.fadeOut()` -  
`.fadeTo()` -  
`.fadeToggle()` -  

SLIDING  
`.slideDown()` -  
`.slideUp()` -  
`.slideToggle()` -  

CUSTOM  
`.delay()` -  
`.stop()` -  
`.animate()` -  

* **Eventos**  

ARCHIVOS - DOCUMENTOS  
`.ready()` -  
`.load()` -  

USUARIO  
`.on()` -  

---






```js

```