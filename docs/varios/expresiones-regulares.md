

## EXPRESIONES REGULARES

* **Flags**

Se añaden despues de la expresion regular para indicar algo

`/expReg/i` - ignora mayusculas o minusculas  
`/expReg/g` - repetir la busqueda a traves de la cadena entera  

* **Basicos**

`.` - cualquier cosa  
`\s` - espacio en blanco  
`\S` - cualquiera menos espacio en blanco  
`\d` - numero  
`\D` - no numero  
`\w` - caracter alfanumerico (a-z, A-Z, 0-9)  
`\W` - no caracter alfanumerico  
`\t` - tabulador  
`\r` - retorno de carro   
`\n` - nueva linea   

* **Limites**

`^` - Comienzo de cadena o de multilinea  
`\A` - Comienzo de cadena  
`$` - Fin de cadena o fin de multilinea   
`\Z` - Fin de cadena  
`\b` - Palabra limite , "\bcat\b" pilla en "cat in the hat" pero no en "locate"  
`\B` - No es una palabra limite  
`\<` - Comienzo de palabra   
`\>` - Fin de palabra  

* **Grupos y rangos**

Los rangos son inclusive  

`(X|Y)` - X o Y, ejemplo \b(cat|dog)s\b  pilla "cats" o "dogs"  
`(...)` - Grupos  
`[abc]` - Rango ("a" ó "b" ó "c")  
`[^abc]` - No ("a" ó "b" ó "c")  
`[a-h]` - Minuscula entre la "a" y la "h"  
`[A-H]` - Mayucula entre la "a" y la "h"  
`[3-6]` - Numero entre 3 y 6 ambos inclusive  

* **Cuantificadores**

Por defecto los cuantificadores solo se aplican a un caracter, para aplicarlo
a mas usar (...) para especificar la cadena buscada  

`X*` - 0 ó mas repeticiones de X  
`X+` - 1 ó mas repeticiones de X  
`X?` - 0 ó instancias de X, vamos que es opcional  
`X{m}` - exactamente m instancias de X  
`X{m,}` - al menos m instancias de X  
`X{m,n}` - entre m y n instancias(incluidas) de X  


* **Especiales**

`{}[]()^$.|*+?\` y `-` entre corchetes deben ser escapados con contrabarra `\`

### Javascript

En Javascript las expresiones regulares han de estar en una sola linea.  
Los espacios en blanco son significativos  
Delimitadas por barras /  

```js
var expreg = /ab+c/;
var expreg = new RegExp('/ab+c/');
expreg.test(variableATestarConLaexpresionRegular) // da true o false
```

* **Metodos**

`regexp.exec(string)` Si la expresion regular casa con el string devuelve un
array  

* array[0] es la subcadena que casa con la expresion regular
* array[1] el texto capturado por el primer grupo
* array[2] el texto capturado por el segundo grupo
* array[n] el texto capturado por el n grupo
* Si la expresion regular tiene una bandera g la busqueda empieza en la posicion regexp.lastIndex (inicialmente cero). Si la busqueda es exitosa regexp.lastIndex pasara a ser el siguiente caracter tras la coincidencia. Si falla pasa a 0.  
* Esto permite buscar varias coincidencias en una sola cadena

`regexp.test(string)` Si la expresion regular casa con el string devuelve
true, si no false
```js
var b = /&.+;/.test('frank & beans'); // b is true
```  

`string.replace(expreg, nueva)` - en la cadena string reemplaza lo que casa con la expresion regular por la string nueva  

`string.match(regexp)` - busca la regexp en la cadena string y devuelve las coincidencias en un array

```js
var str = "The rain in SPAIN stays mainly in the plain";
var res = str.match(/ain/g); 
console.log(res) // [ 'ain', 'ain', 'ain' ]
```

### ExpReg Comunes

`.replace(/\s+/g, "")` - Elimina todos los espacios en blancos  
`.replace(/\s/g, "loQueSea")` - Reemplaza todos los espacios en blanco por
loQueSea   

`/^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/` - direccion de correo valida

[Aqui](http://code.tutsplus.com/tutorials/8-regular-expressions-you-should-know--net-6149)

---
