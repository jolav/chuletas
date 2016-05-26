# JAVASCRIPT 1  

`console.log()`  
`prompt("introduce datos aqui")`  
`confirm("Aceptas si o no")`  
`alert("Ventana en la pantalla")`  

`//` Comentarios de una linea  
`/* ... /*` Comentarios multilinea

---

## TIPOS DATOS

`typeof dato`  
Devuelve una string con el tipo de la variable dato

![js1](/z-static/images/js/tiposDatos.png)

### Numeros

> * Internamente son un numero flotante de 64 bits  
 * `NaN (Not a number)` no equivale a ningun valor ni siquiera a el mismo  

* Metodos :

`isNan(numero)` - Detecta si es numero o no  
`numero.toFixed(n)` - Redondea a n decimales (devuelve una string)   
`numero.toPrecision(n)` - Redondea a n digitos decimales incluidos (devuelve 
una string)  
`numero.toExponential(n)` - Representa el numero en notacion exponencial 
(devuelve una string)    
`numero.toString()`- equivale a `String(numero)` y devuelve una string  

#### Math

`Math` es un Objeto Global que actua sobre numeros 

* Propiedades

`Math.PI` - Devuelve pi 3.14159265359

* Metodos  

`Math.round(num)` - Redondea num al entero mas cercano    
`Math.sqrt(num)` - Devuelve la raiz cuadrada del numero positivo num    
`Math.ceil(num)` - Redondea al entero mas cercano por arriba     
`Math.floor(num)` - Redondea al entero mas cercano por abajo  
`Math.random()` - Genera un numero aleatorio entre 0 (inclusivo) y 1 
(no inclusivo)   

```js
// Crea un numero aleatorio entre 1-10
var aleatorio = Math.floor((Math.random() * 10) +1);
```


### String

>* Puede estar entre comillas simples o comillas dobles  
* Las cadenas tienen un propiedad lenght. `"cadena".lenght`  
* Las cadenas son inmutables. no se pueden cambiar pero si crear nuevas  
* Para escapar caracteres usamos \ `\n` nueva linea por ejemplo  

* Metodos

`parseInt(string)` - Convierte la cadena string en un numero  
`string.charAt(pos)` - Devuelve la letra en la posicion pos  

```js
var name = 'Curly';
var initial = name.charAt(0);       // initial is 'C'
```  

`string.charCodeAt(pos)` - Devuelve el codigo de la letra en la posicion pos
  
```js
var name = 'Curly';
var initial = name.charCodeAt(0);   // initial is 67
```  

`string.concat(str1, str2, ...)` - Junta los strings. Mejor usar +

```js
var s = 'C'.concat('a', 't');       // s es 'Cat'
```

`string.indexOf(cadenaBuscada, pos)` - Busca una cadenaBuscada en la string y si
la encuentra devuelve la posicion del primer caracter, si no devuelve -1.  
El parametro opcional pos indica a partir de donde buscar  

```js
var text = 'Mississippi';
var p = text.indexOf('ss');         // p es 2
p = text.indexOf('ss', 3);          // p es 5
p = text.indexOf('ss', 6);          // p es -1
```

`string.lastIndexOf(cadenaBuscada, pos)` - Como indexOf pero busca desde atras
hacia adelante

```js
var text = 'Mississippi';
var p = text.indexOf('ss');         // p es 5
p = text.indexOf('ss', 3);          // p es 2
p = text.indexOf('ss', 6);          // p es 5
```

`string.replace(valorBuscado, valorNuevo)` - Reemplaza el valorBuscado por el valorNuevo solo una vez salvo que el valorBuscado sea una expresion regular

```js
var str = "Visita mi casa";
var res = str.replace("casa", "huerta"); // "Visita mi huerta"
```

`string.search(regexp)` - Busca una cadena que case con la expresion regular pasada y devuelve la posicion del primer caracter o -1

```js
var text = 'Por la mañana " yo me levanto';
var pos = text.search(/["']/);          // pos es 14
```

`string.slice(comiezo, fin)` - Crea una nueva string desde la posicion comienzo incluida hasta la posicion fin NO incluida

```js
var cadena = 'Hola mundo';
var res = cadena.slice(1, 4);           // res es 'ola'
```

`string.split(separador, limite)` - Crea un array de cadenas al separar el string original en piezas separadas por el parametro separador  
Limite es el limite de nuevas cadenas

```js
var cadena = "Como lo llevas hoy amigo";
var res = cadena.split(" ", 4)  // res es ['Como','lo','llevas','hoy']
```

`string.substring(comienzo, fin)` - Usar `string.slice()` en su lugar  
`string.toLowerCase()` - Convierte la string a minusculas    
`string.toUpperCase()` - Convierte la string a mayusculas    
`string.fromCharCode(car1, car2, ...)`  

```js
var a = String.fromCharCode(67, 97, 116);
// a is 'Cat'
```

`string.trim(cadena)` - Elimina los espacios en blanco del comienzo y del final 
de la cadena  

### Boolean

`true` o `false`

Operadores Logicos

```js
AND --> &&
OR  --> ||
NOT --> !
Operador ternario o operador condicional
test ? expresion1 : expresion2
Si test es true devuelve expresion1, si es falso devuelve expresion2
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

`null` es para objetos  
`undefined` es para propiedades, metodos o variables  

### Aumentando tipos

* Si añadimos un metodo a `Object.prototype` ese metodo esta disponible para
todos los objetos
* En las funciones lo mismo aumentando `Function.prototype`

---

## VISIBILIDAD

### Alcance

La visibilidad es hacia fuera en las funciones. Yo veo las
variables de las funciones externas que me contienen pero no veo las de dentro  

### ES6 let (como var)

Declara una variable que solo se vera en ese bloque donde se ha definido
(funcion, if..else, while)  

### Global

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

### Local

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

## DECISIONES Y BUCLES

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

`inicializacion` - Se ejecuta antes que el bucle empiece  
`condicion` - Define la condicion para seguir ejecutando el bucle  
`actualizacion` - Se ejecuta cada vez que el bucle se ha ejecutado  

```js
for (inicializacion; condicion; actualizacion) {
  codigo_se_ejecuta:con_cada_bucle;
}

for (var i = 0; i < 10; i++) {
  document.writeln(i);
}
```

### for in

Recorre todas las propiedades de un objeto o array

```js
for (variable in [object | array]) {
  instrucciones;
}

for (p in window) {
  document.writeln(p + "<br/>");
}
```

### for each ... in

`DEPRECATED` Recorre todas las propiedades de un objeto

### switch

```js
switch(expresion) {
  case 'uno':
    instrucciones;
    break;
  case 'dos':
    instrucciones;
    break;
  default:
    instrucciones_por_defecto;
    break;
}
```

```js
switch (new Date().getDay()) {   //getDay() numero de dia de la semana
  case 6:
    text = "Sabado";
    break;
  case 0:
    text = "Domingo";
    break;
  default:
    text = "Entre semana";
    break;
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

* **Añadir elementos**

`array.push(item1, item2, ...)` - Añade los items al final del array. Pero cada
 item que añade lo hace como un array item

```js
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.push(b, true);
// a is['a', 'b', 'c', ['x', 'y', 'z'], true]
// c is 5
```

`array.unshift(item1, item2, ...)` - como push pero añade los items al principio
 del array

```js
var a = ['a', 'b', 'c'];
var r = a.unshift('?', '@');
// a es ['?', '@', 'a', 'b', 'c']
//r es 5
```

* **Eliminar elementos**

`array.pop()` - Elimina y devuelve el ultimo elemento del array. Si vacio
devuelve undefined

```js
var a = ['a', 'b', 'c'];
var c = a.pop( ); // a is ['a', 'b'] & c is 'c'
```

`array.shift()` - elimina el primer elemento del array y lo devuelve Si el array esta vacio devuelve undefined

```js
var a = ['a', 'b', 'c'];
var c = a.shift( );
// a es ['b', 'c'] , c is 'a'
```

* **Iteracion**

`forEach()` - Ejecuta una funcion una vez para cada elemento del array.  

```js
var people = [
    {name: 'Casey', rate: 60},
    {name: 'Camille', rate: 80},
    {name: 'Gordon', rate: 75},
    {name: 'Nigel', rate: 120}
];
var results = [];                                
people.forEach(function(person) {                
  if (person.rate >= 65 && person.rate <= 90) {  
    results.push(person);                        
  }
});
```

`some()` - Comprueba si algunos elementos del array pasan un test definido 
por una funcion  

```js
var ages = [3, 10, 18, 20];
function checkAdult(age) {
    return age >= 18;
}
function myFunction() {
    res = ages.some(checkAdult);
}
// res = true
```

`every()` - Comprueba si todos los elementos del array pasan un test definido 
por una funcion  

```js
var ages = [3, 10, 18, 20];
function checkAdult(age) {
    return age >= 18;
}
function myFunction() {
    res = ages.every(checkAdult);
}
// res = false
```

* **Combinar**

`array.concat(item1, item2 ...)` - Va añadiendo los items

```js
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.concat(b, true);
// c is ['a', 'b', 'c', 'x', 'y', 'z', true]
```

* **Filtrar**

`filter()` - Crea un nuevo array con todos los elementos que pasan un test
especificado en una funcion  

```js
var people = [
    {name: 'Casey', rate: 60},
    {name: 'Camille', rate: 80},
    {name: 'Gordon', rate: 75},
    {name: 'Nigel', rate: 120}
];
function priceRange(person) {                        
  return (person.rate >= 65) && (person.rate <= 90); 
};
var results = [];                              
results = people.filter(priceRange);           
```

* **Ordenar**

`array.sort(funcionDeComparacion)` - Por defecto la comparacion la hace
 asumiendo que todos los elementos son strings

```js
var n = [4, 8, 15, 16, 23, 42];
n.sort(function (a, b) {
  return a - b;
});
// n is [4, 8, 15, 16, 23, 42];
```

`array.reverse()` - invierte todos los elementos del array
```js
var a = ['a', 'b', 'c'];
var b = a.reverse( );
// tanto a como b son ['c', 'b', 'a']
```

* **Modificar**

`map()` - Llama una funcion sobre cada elemento del array y crea un nuevo 
array con los resultados  

```js
var numbers = [4, 9, 16, 25];

function myFunction() {
  x = numbers.map(Math.sqrt);
// x = [2, 3, 4, 5]
```

`array.join(separador)` - crea un string concatenando todos los elementos del
array usando el separador indicado que por defecto es ','. Si usas espacio en blanco como separador se unen todos sin separacion

```js
var a = ['a', 'b', 'c'];
a.push('d');
var c = a.join(''); // c is 'abcd';
```

`array.slice(comienzo, fin)` - copia una parte del array desde el array[comienzo] incluido hasta el array[fin] NO incluido. Fin es opcional y por defecto es array.length

```js
var a = ['a', 'b', 'c'];
var b = a.slice(0, 1);  // b es ['a']
var c = a.slice(1); // c es ['b', 'c']
var d = a.slice(1, 2); // d es ['b']
```

`array.splice(comienzo, borrarCont, item1, item2, ...)` - Elimina elementos reemplazandolos por los nuevos item. array[comienzo] a partir de aqui (el incluido) elimina los siguientes borrarCont e inserta los items en su lugar
Devuelve los elementos borrados

```js
var a = ['a', 'b', 'c'];
var r = a.splice(1, 1, 'ache', 'bug');
// a es ['a', 'ache', 'bug', 'c']
// r es ['b']
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
4- El valor de this depende del patron de invocacion de los cuales
existen 4
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

Son funciones que se ejecutan una vez que el proceso asincrono que las llama
 se ha terminado

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

### Contexto de ejecucion

> Se corresponden con el alcance o visibilidad de las variables  

> * `Contexto Global`: codigo que esta en el script pero no en una funcion. 
> Solo hay un contexto global en una pagina    
> * `Contexto de funcion`: codigo que se ejecuta dentro de una funcion. Cada 
> funcion tiene su propio contexto  

> Cada vez que un script entra en un nuevo contexto de ejecucion hay dos 
> fases :

> * `Preparacion`:, se crea un nuevo alcance. Se crean variables, funciones y
> argumentos y se determina el valor de `this`.   
> * `Ejecucion`: ahora se pueden asignar valores a las variables, se 
> referencian las funciones y se ejecuta su codigo y se ejecutan tambien las
> sentencias  

* **Objeto variables**

> * Cada contexto de ejecucion tiene su propio objeto `variables` que tiene 
> las variables, funciones y parametros que estan disponibles para ese 
> contexto.  
> * Cada contxto de ejecucion tiene tambien acceso al padre de objeto ç
>`variables`  

* **Excepciones**

Cuando se produce un error, mira en su contexto a ver si hay codigo para 
manejar el error, si no lo hay sube hacia arriba por el stack buscando codigo 
para manejar el error. Si llega al contexto global y no lo encuentra termina
la ejecucion del script y crea un objeto `Error` 

### Objecto Error

* **Tipos de objetos error**

`Error` - error generico
`SyntaxError` - no se ha respetado la sintaxis  
`ReferenceError` - se ha referenciado una variable que o no esta declarada o 
esta fuera del alcance  
`TypeError` - hay un inesperado tipo de datos que no puede ser forzado  
`RangeError` - numeros en un rango no aceptable  
`URIError` - metodos del tipo encodeURI() decodeURI() mal usados  
`EvalError` - funcion eval() mal usada  

* **Propiedades**

`name` - tipo de ejecucion  
`message` - descripcion  
`fileNumber` - nombre del archivo javascript  
`lineNumber` - numero de la linea con error  

### Depuracion

> * **¿ Donde esta el fallo ?**

1. Hay que acotar al maximo el area donde esta el problema.

2. El mensaje de error te dice:  
    * el script donde esta el problema  
    * el numero de linea donde el script ya no puede continuar. Lo normal es
      que el fallo este antes
    * el tipo de error
3. Prueba a ver hasta donde se ejecuta el script uando mensajes en la consola
para ello  
4. Usa breakpoints para parar la ejecucion e inspeccionar los valores 
almacenados en las variables  

> * **¿ Cual es el fallo ?**

1. En los breakpoints si ves valores normales busca antes
2. Parte el codigo en partes mas pequeñas para testearlas  
    * usa la consola para escribir los valores de las variables
    * llama a las funciones desde la consola para ver si devuelven lo que se
    espera de ellas  
    * comprueba si los objetos existen y tienen los metodos y propiedades 
    que se espera que tengan  
3. Comprueba el numero de parametros de una funcion o el numero de elementos 
de un array.

> * **Trucos**

* Prueba otro navegador, a veces los errores son especificos del navegador   
* Manda numeros a la consola a ver cuando se para la ejecucion del programa  
* Comenta partes del codigo para ir acotando zonas
* StackOverflow es tu amigo  
* Herramientas de validacion de codigo online:
    * [jslint](http://jslint.com)
    * [jshint](http://jshint.com)
    * [jsonlint](http://jsonlint.com)

### Manejo excepciones 

Interrumpen la ejecucion del programa. Para evitarlo hay que capturarlas

```js
try {
  // intenta ejecutar este codigo
} catch (exception) {
  // si hay una excepcion ejecuta este codigo
} finally {
  // esto siempre se ejecuta
}
```

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

### Throwing errors

Si sabes que algo podria causar un fallo puedes generar tus propios errores 
antes que el interprete lo haga.  

`throw new Error("mensaje");`

```js
var width = 12;                                     
var height = 'test';                              
function calcArea(width, height) {
  try {
    var area = width * height;                      
    if (!isNaN(area)) {                   // If it is a number
      return area;                                  
    } else {                              // Otherwise throw an error
      throw new Error('calcArea() received invalid number');
    }
  } catch(e) {                            // If there was an error
    console.log(e.name + ' ' + e.message);          
    return 'We were unable to calculate the area.'; 
  }
}
document.getElementById('area').innerHTML = calcArea(width, height);
```

---

## PATRONES

### Reducir variables globales

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
	// Nos da acceso a las globales jQuery (as $) y YAHOO aqui dentro
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

### Datos: Arrays VS Objects

> Grupos de obejtos se pueden almacenar en arrays o como propiedades de otros 
> objetos  

* **Objetos en un array**

```js
var people = [
  {name: 'Casey ', rate: 70, active: true},
  {name: 'Camille', rate: 80, active: true},  
  {name: 'Gordon', rate: 75, active: false},
  {name: 'Nigel', rate: 120, active: true}
]
```

* **Objetos como propiedades de otros objetos**

```js
var people = {
  Casey= {rate: 70, active: true},
  Camille = {rate: 80, active: true}, 
  Gordon= {rate: 75, active: false} ,
  Nigel = {rate: 120, active: true }
}
```


---

