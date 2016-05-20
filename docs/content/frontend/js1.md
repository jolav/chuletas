# JAVASCRIPT 1

---

## TIPOS DATOS

`typeof dato`  
Devuelve una string con el tipo de la variable dato

![js1](/z-static/images/js/tiposDatos.png)

### Numeros

* Internamente son un numero flotante de 64 bits  
* `NaN (Not a number)` no equivale a ningun valor ni siquiera a el mismo  
* Se detectan con `isNan(numero)`
* `Math` es un objeto con metodos que actuan sobre numeros pej:
`Math.floor(num)` que redondea al entero mas cercano por abajo  
* `number.toString()` equivale a `String(number)`  

```js
Math.PI.toString();
3.141592653589793
```

### String

* Puede estar entre comillas simples o comillas dobles  
* Las cadenas tienen un propiedad lenght. `"cadena".lenght`  
* Las cadenas son inmutables. no se pueden cambiar pero si crear nuevas  
* Para escapar caracteres usamos \ `\n` nueva linea por ejemplo  

`parseInt(string)` Convierte la cadena string en un numero

`string.charAt(pos)` Devuelve la letra en la posicion pos

```js
var name = 'Curly';
var initial = name.charAt(0);       // initial is 'C'
```

`string.charCodeAt(pos)` Devuelve el codigo de la letra en la posicion pos

```js
var name = 'Curly';
var initial = name.charCodeAt(0);   // initial is 67
```

`string.concat(str1, str2, ...)` Junta los strings. Mejor usar +

```js
var s = 'C'.concat('a', 't');       // s es 'Cat'
```

`string.indexOf(cadenaBuscada, pos)` Busca una cadenaBuscada en la string y si
la encuentra devuelve la posicion del primer caracter, si no devuelve -1.  
El parametro opcional pos indica a partir de donde buscar

```js
var text = 'Mississippi';
var p = text.indexOf('ss');         // p es 2
p = text.indexOf('ss', 3);          // p es 5
p = text.indexOf('ss', 6);          // p es -1
```

`string.lastIndexOf(cadenaBuscada, pos)` Como indexOf pero busca desde atras
hacia adelante

```js
var text = 'Mississippi';
var p = text.indexOf('ss');         // p es 5
p = text.indexOf('ss', 3);          // p es 2
p = text.indexOf('ss', 6);          // p es 5
```

`string.replace(valorBuscado, valorNuevo)` reemplaza el valorBuscado por el valorNuevo solo una vez salvo que el valorBuscado sea una expresion regular

```js
var str = "Visita mi casa";
var res = str.replace("casa", "huerta"); // "Visita mi huerta"
```

`string.search(regexp)` Busca una cadena que case con la expresion regular pasada y devuelve la posicion del primer caracter o -1

```js
var text = 'Por la mañana " yo me levanto';
var pos = text.search(/["']/);          // pos es 14
```

`string.slice(comiezo, fin)` Crea una nueva string desde la posicion comienzo incluida hasta la posicion fin NO incluida

```js
var cadena = 'Hola mundo';
var res = cadena.slice(1, 4);           // res es 'ola'
```

`string.split(separador, limite)` Crea un array de cadenas al separar el string original en piezas separadas por el parametro separador  
Limite es el limite de nuevas cadenas

```js
var cadena = "Como lo llevas hoy amigo";
var res = cadena.split(" ", 4)  // res es ['Como','lo','llevas','hoy']
```

`string.substring(comienzo, fin)` usar `string.slice()` en su lugar

`string.toLowerCase()` Convierte la string a minusculas  

`string.toUpperCase()` Convierte la string a mayusculas  

`String.fromCharCode(car1, car2, ...)`  

```js
var a = String.fromCharCode(67, 97, 116);
// a is 'Cat'
```

### Boolean

`true` o `false`

Operadores Logicos

```js
AND --> &&
OR  --> ||
NOT --> !
Operador ternario o operador condicional
test ? expresion1 : expresion2
Si test es true devuelve expresion1, si test es falso devuelve expresion2
```

Cuando convertimos un no-booleano a booleano es falso o true?

```js
-- Falso
"" cadena Vacia
0, -0, NaN
null, undefined
false
-- Todo lo demas es true
```

### Null - Undefined

Se usan para denotar la ausencia de valor  
Muchas opeciones en JS que no producen un valor significativo dan undefined sencillamente porque un valor tienen que dar  
La diferencia entre ellos es irrelevante, se pueden tratar igual

### Aumentando tipos

* Si añadimos un metodo a `Object.prototype` ese metodo esta disponible para
todos los objetos
* En las funciones lo mismo aumentando `Function.prototype`

### Ambito o visibilidad

#### Alcance

La visibilidad es hacia fuera en las funciones. Yo veo las
variables de las funciones externas que me contienen pero no veo las de dentro  

#### ES6 let (como var)

Declara una variable que solo se vera en ese bloque donde se ha definido
(funcion, if..else, while)  

#### Global

* Declarada fuera de una funcion
```js
var carName = 'volvo';
// codigo aqui puede usar carName
function myFunction() {
  // codigo aqui puede usar carName
}
```

* Si asigno un valor a una variable no declarada, esta se convierte en global
```js
// codigo aqui puede usar carName
function myFunction() {
  carName = 'volvo';
  // codigo aqui puede usar carName
}
```

#### Local

Variables declaradas dentro de una funcion permanecen locales a esa funcion

```js
// codigo aqui NO puede usar carName
function myFunction() {
  var carName = 'volvo';
  // codigo aqui puede usar carName
}
```

---

## OPERADORES

### Aritmeticos

* `+` Suma
* `-` Resta
* `*` Multiplicacion
* `/` Division
* `%` Modulo, lo que sobra de la division entera
* `++` Incremento
* `--` Decremento

### Asignacion

* `=` x = y
* `+=` x = x + y
* `-=` x = x - y
* `*=` x = x * y
* `/=` x = x / y
* `%=` x = x % y

### Comparacion

* `==` igual
* `===` igual valor e igual tipo
* `!=` no igual
* `!==` no igual valor ni igual tipo
* `>` mayor que
* `<` menor que
* `<=` mayor o igual que
* `>=` menor o igual que

### Logicos

* `&&` AND
* `||` OR
* `!` NOT

### Unitarios

* `typeof()` Nos da el tipo de valor

---

## ESTRUCTURA

* Expresiones: fragmentos de codigo que producen un valor
* Instrucciones: lineas que acaban con punto y coma ;

`console.log()`  
`prompt("introduce datos aqui")`  
`confirm("Aceptas si o no")`  
`alert("Ventana en la pantalla")`  

`//` Comentarios de una linea  
`/* ... /*` Comentarios multilinea


### if ... else

```js
if (condicion) {
  instrucciones_if_condicion_es_true;
else {
  instrucciones_if_condicion_es_false;
}
```

En cascada

```js
if (condicion1) {
  instrucciones_if_condicion1_es_true;
} else if (condicion2) {
  instrucciones_if_condicion2_es_true;
} else id (condicion3) {
  instrucciones_if_condicion3_es_true;
} ...
else {
  instrucciones_if_ninguna_condicion_es_true
}
```


### while

`while` El codigo puede que no se ejecute nunca

```js
while (condicion) {
  instrucciones_while_condicion_es_true;
}
```

`do while` El codigo se ejecuta minimo una vez

```js
do {
  instrucciones_while_condicion_es_true
while (condicion)

```

### for

```js
instruccion1 - se ejecuta antes que el bucle empiece
instruccion2 - define la condicion para seguir ejecutando el bucle
instruccion3 - se ejecuta cada vez que el bucle se ha ejecutado
for (instruccion1; instruccion2; instruccion3) {
  codigo_se_ejecuta:con_cada_bucle;
}
```

### for in

Recorre todas las propiedades de un objeto o array

```js
for (variable in [object | array]) {
  instrucciones;
}
```

### for each ... in

`DEPRECATED` Recorre todas las propiedades de un objeto

### switch

```js
switch(expresion) {
  case n:
    instrucciones;
    break;
  case n:
    instrucciones;
    break;
  default:
    instrucciones_por_defecto;
}
```

```js
switch (new Date().getDay()) {   //getDay() da el numero de dia de la semana
  case 6:
    text = "Sabado";
    break;
  case 0:
    text = "Domingo";
    break;
  default:
    text = "Entre semana";
}
```

---

## OBJETOS

Un Objeto es un contenedor de propiedades  
Una propiedad es un nombre y un valor (cualquiera menos undefined)  
Los objetos se pasan siempre por `referencia`  

### this

Usada dentro de funciones y objetos para referirse a un objeto que lo normal 
es el objeto en el que la funcion opera

* Funcion con alcance global (no esta dentro de ningun otro objeto o funcion) `this` se refiere al objeto `window`

```js
function windowSize() {
  var width = this.innerWidth;
  var height = this.innerHeight;
  return [height, width]
}
```

* Variables globales, las variables globales se convierten en propiedades del 
objeto `window`

```js
var width = 600;
var shape = {width: 300};
var showWidth = function() {
  document.write(this.width);
};
showWidth();      // this.width = 600
```

* Metodo de un objeto, si una funcion se define dentro de un objeto se 
convierte en un metodo de ese objeto. En un metodo `this` se refiere al objeto
que lo contiene

```js
var shape = {
  width: 600,
  height: 400,
  getArea: function() {
    return this.width * this.height;
  }  
}
```

* Funciones con nombre globales y usadas como metodos de un objeto, `this` se
refiere al objeto que contiene a la funcion

```js
var width = 600;
var shape = {width: 300);
var showWidth = function() {
  document.write(this.width);
};
shape.getWidth = showWidth;
shape.getWidth();                 // this.width = 300
```

### Crear Objetos

#### Literales

* Los valores de las propiedades se pueden obtener de cualquier expresion
incluso de otros objetos anidados  
* Bueno para la creación de nuevos valores de objeto  
* NO son reusables  

```js
var objetoVacio = {};

var miObjeto = {
  diHola : function() {
    alert("Hola");
  },
  name : "Pepe",
  edad : 50,
  nacimiento : {               // los objetos se pueden anidar
    fecha: "01-01-1980",
    lugar: "La Tierra"
  }
};
```

#### Constructor

NO USAR

* Normal

```js
var persona = new Object();
persona.nombre = 'Ivan';
persona.edad = '50';
```

* Reusable

```js
function myPersona (a, b) {
  this.name = a;
  this.edad = b;
}
var actor1 = new myPersona ('Juan', '30');
var actor2 = new myPersona ('Manu', '18');
```

### Prototipos

`Object.create` crea un nuevo objeto que usa uno viejo como prototipo

```js
var proto = {
  sentencia : 4,
  condicional : 2
}
var creaPrisionero = function (nombre, id) {
  var prisionero = Object.create(proto);
  prisionero.nombre = nombre;
  prisionero.id = id;
  return prisionero;
}
var primerPrisionero = creaPrisionero ('Juan', '65891');
var segundoPrisionero = crearPrisionero ('Pepe', '13698');
```

### Enumeracion

* `for in` itera sobre todas las propiedades del objeto incluso funciones o propiedades de los prototipos    
* Para evitarlo se usan filtros como `typeof !== 'function'` ó `hasOwnProperty`  
* Tambien puede usar el `for`  

### Metodos

`object.hasOwnProperty(nombre)` Devuelve true si el objeto tiene la propiedad nombre y false si no. El prototipo no se examina

```js
var a = {member: true};
var b = Object.create(a);
var t = a.hasOwnProperty('member'); // t is true
var u = b.hasOwnProperty('member'); // u is false
var v = b.member                    // v is true
```

---

## ARRAYS

Los arrays en Javascript son falsos arrays pero aun asi renta su uso

* Creacion de Arrays

* * Array Literal  

```js
var vacio = [];
var numeros = ['cero','uno','dos','tres','cuatro','cinco',
               'seis','siete','ocho','nueve','diez'];
vacio[1]    // undefined
numeros[1]  // 'one'
```

* * Array constructor

```js
var colores = new Array('blanco', 'rojo', 'azul')
```

Heredan de `Array.prototype` muchos metodos muy utiles

* Iteracion de arrays

```js
No usar for in que da muchos problemas por no ser objetos puros
for ( var i = 0; i < numeros.length; i = i + 1) {
  console.log(numeros[i]);
}
```

### Metodos

`array.concat(item1, item2 ...)` Va añadiendo los items

```js
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.concat(b, true);
// c is ['a', 'b', 'c', 'x', 'y', 'z', true]
```

`array.join(separador)` crea un string concatenando todos los elementos del
array usando el separador indicado que por defecto es ','. Si usas espacio en blanco como separador se unen todos sin separacion

```js
var a = ['a', 'b', 'c'];
a.push('d');
var c = a.join(''); // c is 'abcd';
```

`array.pop()` Elimina y devuelve el ultimo elemento del array. Si vacio
devuelve undefined

```js
var a = ['a', 'b', 'c'];
var c = a.pop( ); // a is ['a', 'b'] & c is 'c'
```

`array.push(item1, item2, ...)` Añade los items al final del array. Pero cada
 item que añade lo hace como un array item

```js
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.push(b, true);
// a is['a', 'b', 'c', ['x', 'y', 'z'], true]
// c is 5
```

`array.reverse()` invierte todos los elementos del array
```js
var a = ['a', 'b', 'c'];
var b = a.reverse( );
// tanto a como b son ['c', 'b', 'a']
```

`array.shift()` elimina el primer elemento del array y lo devuelve Si el array esta vacio devuelve undefined

```js
var a = ['a', 'b', 'c'];
var c = a.shift( );
// a es ['b', 'c'] , c is 'a'
```

`array.slice(comienzo, fin)` copia una parte del array desde el array[comienzo] incluido hasta el array[fin] NO incluido. Fin es opcionale y por defecto es array.length

```js
var a = ['a', 'b', 'c'];
var b = a.slice(0, 1);  // b es ['a']
var c = a.slice(1); // c es ['b', 'c']
var d = a.slice(1, 2); // d es ['b']
```

`array.sort(funcionDeComparacion)` Por defecto la comparacion la hace asumiendo que todos los elementos son strings

```js
var n = [4, 8, 15, 16, 23, 42];
n.sort(function (a, b) {
  return a - b;
});
// n is [4, 8, 15, 16, 23, 42];

```

`array.splice(comienzo, borrarCont, item1, item2, ...)` Elimina elementos reemplazandolos por los nuevos item. array[comienzo] a partir de aqui (el incluido) elimina los siguientes borrarCont e inserta los items en su lugar
Devuelve los elementos borrados

```js
var a = ['a', 'b', 'c'];
var r = a.splice(1, 1, 'ache', 'bug');
// a es ['a', 'ache', 'bug', 'c']
// r es ['b']

```

`array.unshift(item1, item2, ...)` como push pero añade los items al principio
 del array

```js
var a = ['a', 'b', 'c'];
var r = a.unshift('?', '@');
// a es ['?', '@', 'a', 'b', 'c']
//r es 5
```

---

## FUNCIONES

### Funciones como objetos

* Las funciones son enlazadas a `Function.prototype` que a su vez esta enlazado
 a `Object.prototype`  
* Cada funcion se crea con una propiedad prototype cuyo valor es un objeto con
 una propiedad constructor cuyo valor es la funcion  
* Cada funcion se crea con dos propiedades ocultas  
* * El contexto de la funcion
* * El codigo que implementa el comportamiento de la funcion

### Tipos de funciones

#### Named functions

  * Declaracion de funciones (function declaration) :las puedes llamar cuando quieras antes de que se lean, incluso en el onload

```js
function nombreFuncion () {
  codigo;
}
```

  * expresiones de funciones (function expressions) :  no se pueden usar hasta que este encontrada y definida

```js
ANONIMAS
var nombreFuncion = function() {
  codigo;
}
NAMED
var referenciaFuncion = function nombreFuncion() {
  codigo;
}
```

#### Anonymous functions

```js
function () {
  codigo;
}
```

#### Autoejecutables

Immediately Invoked Function Expressions (IIFEs)  
Una vez declarada se llama a si misma para inicializarse y estar ya disponible para otras partes de la aplicacion, contiene la visibilidad de las variables


```js
ANONIMA
(function(){
	//código de tu función
}());
// con parametros
(function(w,d,o){
    //Ahora w es un alias (shortcut) para window
    //d es un alias de document
    //o es un alias de otraVariableMuyLarga
}(window, document, otraVariableMuyLarga));
```

```js
NAMED
(function nombreFuncion(){
	//código de tu función
}());
```

### Invocacion

* Se realiza `nombrefuncion();`

Cuando invocamos una funcion

```js
1- Se suspende la ejecucion de la funcion actual
2- Pasa el control y los parametros a la nueva funcion
3- Tambien se pasan this y arguments
4- El valor de this depende del patron de invocacion de los cuales existen 4
```

Patrones de invocacion

```js
- Metodos, this se refiere al objecto desde el que se invoca el metodo
- Funcion, this se refiere al objeto global
- Constructor, this se refiere al nuevo objeto que se esta construyendo
- Apply PENDIENTE
```

### Parametros y Argumentos

Pequeña diferencia entre ambos

* Parametros: son las variables que se definen cuando se declara la funcion
* Argumentos: son los valores que se pasan a la funcion al invocarla
* Si a una funcion le pasamos demasiados parametros los que sobran los ignora  
* Si a una funcion le pasamos menos parametros a los que faltan les asigna  undefined

### return multiples valores

Para ello usan un array

```js
funcion getMedidas(anchura, altura, profundidad) {
  var area = anchura * altura;
  var volumen = anchura * altura * profundidad;
  var medidas = [area, volumen];
  return medidas;
}
var area1 = getMedidas(5,4,10)[0];
var volumen1 = getMedidas(5,4,10)[1];
```

### Closure

Sin closure

```js
var cont = 0;
function contador() {
  cont = cont + 1;
  return cont;
}
```

Closure ejemplo1

```js
function crearContador() {
  var cont = 0;
  function contador() {
    cont = cont + 1;
    return cont;
  }
  return contador
}
```

Closure ejemplo2

```js
function wrapValue(n) {
  var localVariable = n;
  return function() {
    return localVariable;
  };
}
var wrap1 = wrapValue(1);
var wrap2 = wrapValue(2);
console.log(wrap1());         // → 1
console.log(wrap2());         // → 2
```

### Callback

Son funciones que se ejecutan una vez que el proceso asincrono que las llama se ha terminado

```js
function nombreCompleto (nombre, apellido, callback) {
  console.log("Me llamo " + nombre + " " + apellido);
  callback(apellido);
}
var saludos = function (ape) {
  console.log("Hola, Sr " + ape);
};
nombreCompleto("Brus", "Bilis", saludos);
```

### Recursividad

Una funcion que se llama a si misma es recursiva

```js
function power(base, exponent) {
  if (exponent == 0)
    return 1;
  else
    return base * power(base, exponent - 1);
}
console.log(power(2, 3));              // → 8
```

---

## HERENCIA

* En javascript los objetos heredan directamente de otros objetos (prototipos)  
* Con el operador `new` podemos montarnos factorias de objetos y hacer un pseudoclasico patron de diseño

---

## EXPRESIONES REGULARES

En Javascript las expresiones regulares han de estar en una sola linea. Los espacios en blanco son significativos  

```js
var expreg = /ab+c/;
var expreg = new RegExp('/ab+c/');
expreg.test(variableATestarConLaexpresionRegular) // da true o false
```

### Metodos

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

---

## ERRORES

### Excepciones

Interrumpen la ejecucion del programa. Para evitarlo hay que capturarlas

```js
var intentalo = function ( ) {
  try {
    add("adelante");
  } catch (exception) {
    document.writeln(exception.name + ': ' + exception.message);
  }
}
intentalo( );
```

---

## PATRON DE MODULOS

### Reduccion de variables globales

`Closures anonimos` Son funciones autoejecutables

```js
(function () {
  // todas las variables y funciones son locales a esto
  // pero aun tiene acceso a todas las globales
}());
```

`Importacion global`

```js
(function ($, YAHOO) {
	// ahora tenemos acceso a las globales jQuery (as $) and YAHOO aqui dentro
}(jQuery, YAHOO));
```

`var MiAPP = {}` y la convierto en el contenedor de todo para mi aplicacion.
```js
MiAPP.objetoVacio = {};

MiAPP.miObjeto = {
  diHola = function() {
    alert("Hola");
  },
  name: "Pepe",
  edad: 50,
  nacimiento: {               // los objetos se pueden anidar
    fecha: "01-01-1980",
    lugar: "La Tierra"
  }
};
```

### Exportar modulo


`Exportacion del modulo` Exportamos el modulo con 2 propiedades publicas (las mod.algo), el resto se mantiene como privado.  
Podemos importar facilmente las globales que necesitemos usando el patron
anterior

```js
var Modulo = (function () {
	var mod = {};
	var privadaVariable = 1;
	function privadoMetodo() {
		// ...
	}
	mod.moduloPropiedad = 1;
	mod.moduloMetodo = function () {
		// ...
	};
	return mod;
}());

```

---

