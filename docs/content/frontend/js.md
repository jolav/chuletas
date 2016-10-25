# JAVASCRIPT  

`console.log()`  
`prompt("introduce datos aqui")`  
`confirm("Aceptas si o no")`  
`alert("Ventana en la pantalla")`  

`//` Comentarios de una linea  
`/* ... /*` Comentarios multilinea

---

## OPERADORES

* **Aritmeticos**

> `+` Suma  
> `-` Resta  
> `*` Multiplicacion  
> `/` Division  
> `%` Modulo, lo que sobra de la division entera  
> `++` Incremento  
> `--` Decremento  

* **Asignacion**

> `=` x = y  
> `+=` x = x + y  
> `-=` x = x - y  
> `*=` x = x * y  
> `/=` x = x / y  
> `%=` x = x % y  

* **Comparacion**

> `==` igual  
> `===` igual valor e igual tipo  
> `!=` no igual  
> `!==` no igual valor ni igual tipo  
> `>` mayor que  
> `<` menor que  
> `<=` mayor o igual que  
> `>=` menor o igual que  

* **Logicos**

> `&&` AND  
> `||` OR  
> `!` NOT  

* **Unitarios**

> `typeof()` - Nos da el tipo de valor  

> `in` - Devuelve true si la propiedad especificada existe en el objeto
especificado  
>> `propiedadNombreOrNumero in nombreObjeto`  
>> `propiedadNombreOrNumero` es una string que representa al nombre de una
 propiedad o un numero que es el indice de un array  
>> `nombreObjeto` es el nombre de un objeto  

```js
var trees = new Array("redwood", "bay", "cedar", "oak", "maple");
0 in trees;        // returns true
3 in trees;        // returns true
6 in trees;        // returns false
"bay" in trees;    // returns false (you must specify the index number,
                   // not the value at that index)
"length" in trees; // returns true (length is an Array property)
```

---

## VARIABLES  

En javascript las variables no tienen tipos, son los valores quienes tienen tipos  

### Alcance

La visibilidad es hacia fuera en las funciones. Yo veo las
variables de las funciones externas que me contienen pero no veo las de dentro  

**Global**

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

**Local**

Variables declaradas dentro de una funcion permanecen locales a esa funcion

```js
// codigo aqui NO puede usar carName
function myFunction() {
  var carName = 'volvo';
  // codigo aqui puede usar carName
}
```

### Conversion de tipos

`Number(string)` - Convierte cadena string en un numero    
`parseInt(string)` - Convierte la cadena string en un numero entero  
`parseFloat(string)` - Convierte la cadena string en un numero flotante  
`String(numero)` - Convierte el numero en una string  
`numero.toString()` - equivale a String(numero) y devuelve una string  

---

## DATOS BASICOS

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

> * Puede estar entre comillas simples o comillas dobles  
> * Las cadenas tienen un propiedad length. `"cadena".length`    
> * Las cadenas son inmutables. no se pueden cambiar pero si crear nuevas  
> * Para escapar caracteres usamos \ `\n` nueva linea por ejemplo  

* Metodos

`parseInt(string)` - Convierte la cadena string en un numero  
`String.fromCharCode()` - Convierte un codigo de letra en una cadena con la
letra

```js
var codigo = 65;
var letra = String.fromCharCode(codigo);      // letra = "A"
```

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

`string.trim(cadena)` - Elimina los espacios en blanco y los saltos de linea del comienzo y del final de la cadena    

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
Muchas operaciones en JS que no producen un valor significativo dan undefined
sencillamente porque un valor tienen que dar  

`null` es para objetos  
`undefined` es para propiedades, metodos o variables  

---

## ESTRUCTURAS DE CONTROL  

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

## ARRAYS

Los arrays en Javascript son falsos arrays pero aun asi renta su uso  
Heredan de `Array.prototype` muchos metodos muy utiles  

* Creacion de Arrays

> * Array Literal  

```js
var vacio = [];
var numeros = ['cero','uno','dos','tres','cuatro','cinco',
               'seis','siete','ocho','nueve','diez'];
vacio[1]    // undefined
numeros[1]  // 'one'
```

> * Array constructor  

```js
var colores = new Array('blanco', 'rojo', 'azul')
```


* Iteracion de arrays

```js
No usar for in que da muchos problemas por no ser objetos puros
for ( var i = 0; i < numeros.length; i = i + 1) {
  console.log(numeros[i]);
}
```

* Lazy initialization

En arrays multidimensionales, el segundo array no se crea por defecto y hay que
inicializarlo antes de usarlo

```js
var matriz = [];
for (var numFila = 0; numFila < filas; numFila++) {
  // lazy initialization
  if (!matriz[numFila]) { matriz[numFila] = []; }
  for (var numColumna = 0; numColumna < cols; numColumna++) {
    matriz[numFila][numColumna] = data[indice + numFila][numColumna];
  }
}
```

```js
var cols = canvas.width / 10;
var rows = canvas.height / 10;
for (var i = 0; i < cols; i++) {
  status[i] = [];
  for (var j = 0; j < rows; j++) {
    status[i][j] = 1;
  }
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
El callback se invoca con tres elementos  
1. EL valor del elemento  
2. El indice del elemento
3. El objeto Array que estamos filtrando  

```js
result = result.filter(function (val, i, res) {
  console.log(val, i, res);
});
```

`reduce()` - Reduce el array a un solo valor. Ejecuta una funcion suministrada
para cada valor del array(de izda a derecha). El valor de return se guarda en
un acumulador  

```js
var total = [0, 1, 2, 3].reduce(function(a, b){ return a + b; });
// total == 6

var numbers = [65, 44, 12, 4];
function getSum(total, num) {
    return total + num;
}
console.log(numbers.reduce(getSum));   // resultado = 125

var array = [4,5,6,7,8];
var singleVal = 0;
 singleVal = array.reduce(function(previousVal, currentVal) {
  return previousVal + currentVal;
}, 0);
```

* **Buscar**

`array.indexOf("elemento")` - Busca el elemento en el array y devuelve su posicion o -1 si no lo encuentra

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");
// a = 2
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

//Para borrar por ejemplo el elemento con indice = 3 de un array
array.splice(indice, 1);
```



---

## FUNCIONES

### Funciones como objetos

* Las funciones son enlazadas a `Function.prototype` que a su vez esta enlazado
 a `Object.prototype`  
* Cada funcion se crea con una propiedad prototype cuyo valor es un objeto con
 una propiedad constructor cuyo valor es la funcion  
* Cada funcion se crea con dos propiedades ocultas
    * El contexto de la funcion
    * El codigo que implementa el comportamiento de la funcion
* Para evitar callback hell y mas faunas mejor usar siempre las funciones todas
con nombre  

### Tipos de funciones

* **Named functions**

> * Declaracion de funciones (function declaration) :las puedes llamar cuando quieras antes de que se lean, incluso en el onload

```js
function nombreFuncion () {
  codigo;
}
```

> * Expresiones de funciones (function expressions) :  no se pueden usar hasta
que este encontrada y definida

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

* **Anonymous functions**

```js
function () {
  codigo;
}
```

* **Autoejecutables**  

Immediately Invoked Function Expressions (IIFEs)  
Una vez declarada se llama a si misma para inicializarse y estar ya disponible
para otras partes de la aplicacion     
Se usa para declarar variables que no afectan al resto del codigo fuera de la funcion pues contiene la visibilidad de las variables  
Pueden usar tambien `return`  


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

`arguments object` - Un objeto similar a un Array que se corresponde con los argumentos pasados a la función.  

```js
function fourArguments (a, b, c) {
  console.log(arguments.length);
  console.log(arguments[0]);
  console.log(arguments[3]);
}
fourArguments('uno', 'dos', 'tres', 'cuatro');
```

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

Es una forma de "recordar" y tener acceso a las variables de una funcion una vez
que ya ha terminado de ejecutarse.   

Closure ejemplo1

```js
function crearContador(x) {
  function sumarCantidad(y) {
    return x + y;
  }
  return sumarCantidad
}
var sumarUno = crearContador(1);
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

```js
function doSomething(callback) {
    // ...

    // Call the callback
    callback('stuff', 'goes', 'here');
}

function foo(a, b, c) {
    // I'm the callback
    alert(a + " " + b + " " + c);
}

doSomething(foo);
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

## OBJETOS

Un Objeto es un contenedor de propiedades  
Una propiedad es un nombre y un valor (cualquiera menos undefined)  
Los objetos se pasan siempre por `referencia`  


### Crear Objetos

* **Literales**

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

* **Constructor**

```js
// NORMAL

var persona = new Object();
persona.nombre = 'Ivan';
persona.edad = '50';
```

```js
// REUSABLE

function myPersona (a, b) {
  this.name = a;
  this.edad = b;
}
var actor1 = new myPersona ('Juan', '30');
var actor2 = new myPersona ('Manu', '18');
```

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

### Herencia

* En javascript los objetos heredan directamente de otros objetos (prototipos)  
* Con el operador `new` podemos montarnos factorias de objetos y hacer un pseudoclasico patron de diseño

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
> * Cada contxto de ejecucion tiene tambien acceso al padre de objeto
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
`RangeError` - numeros en un rango no aceptable  +0
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

```js
function promptDirection (question) {
  var result = prompt(question, '');
  if (result.toLowerCase() == 'left') return 'L';
  if (result.toLowerCase() == 'right') return 'R';
  throw new Error('Invalid direction: ' + result);
}

function look () {
  if (promptDirection('Which way?') == 'L')
    return 'a house';
  else
    return 'two angry bears';
}

try {
  console.log('You see', look());
} catch (error) {
  console.log('Something went wrong: ' + error);
}
```

---

## STRICT MODE

Se activa poniendo `"use strict"` al comienzo del archivo o del cuerpo de una
funcion

- No permite usar variables no declaradas  
- No se puede borrar una variable u objeto usando `delete`    
- No se puede definir la misma propiedad dos veces  
- No permite los nombres de parametros duplicados  
- En una funcion si no se conoce `this` en lugar de usar el objeto global
`window` sera `undefined`  
- Las variables instanciadas dentro del contexto de eval() sólo son válidas en ese contexto.   
- La sentencia `with(){}` ya no se usa  
- Y mas ...  

**js en cliente**

Envolvemos todo la parte del codigo js en una IIFEs

```js
(function() { 'use strict'; /* code here */

.. codigo

}());
```

**node.js**

añadimos al comienzo del archivo

```js
/*jslint node: true */
'use strict';
```

---

## MODULOS

[Modulos](/backend/nodejs/#modulos)

---

## PATRONES

### Reducir variables globales

`funciones autoejecutables IIFEs`  

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

## ES6

### const - let

* **const**

Crea constantes que no se pueden modificar, ha de asignarse el valor en la declaracion   

* **let**

Declara variables que no son accesibles mas alla del ambito de creacion  
Ejemplo mejor: para los `for`    

### funcion arrow

Para las funciones anonimas

```js
// ES5
var multiplicar = function(x, y) {
  return x * y;
}
// ES6
var multiplicar = (x, y) => {
  return x * y
};
```

### template strings

```js
let nombre1 = "JavaScript";  
let nombre2 = "awesome";  
console.log(`Sólo quiero decir que ${nombre1} is ${nombre2}`);
```

### modulos

bar.js

```js
function hello(who) {
    return "Let me introduce: " + who;
}
export hello;
```

foo.js

```js
import hello from "bar";      // import only `hello()` from "bar" module
var hungry = "hippo";
function awesome() {
    console.log(
        hello( hungry ).toUpperCase()
    );
}
export awesome;
```

baz.js

```js
module foo from "foo";      // import the entire "foo" and "bar" modules
module bar from "bar";
console.log(
    bar.hello( "rhino" )
);                             // Let me introduce: rhino
foo.awesome();                 // LET ME INTRODUCE: HIPPO
```

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

#### [String](/frontend/js/#string)

#### [Numero](/frontend/js/#numeros)

#### [Math](/frontend/js/#math)

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

![js2](/z-static/images/js/dom.png) ![js2](/z-static/images/js/nearbyNodes.png)  

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
document.getElementById("one").firstChild.nextSibling.nodeValue;
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

```js
var button = document.querySelector("button");
button.addEventListener("mousedown", function(event) {
  if (event.which == 1)
    console.log("Left button");
  else if (event.which == 2)
    console.log("Middle button");
  else if (event.which == 3)
    console.log("Right button");
});
```

### Event FLow

> Describe el orden en el que los eventos se procesan  
> `Propagacion` : Los eventos registrados en nodos con hijos reciben algunos
> eventos que ocurren en los hijos. Si pulsamos un buton dentro de un parrafo
> los manejadores de evento del parrafo tambien recibiran el evento click. Si
> los dos tienen el evento el manejador mas especifico (el mas interno) va
> primero    

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

### Setting Timers

`setTimeout`: similar a `requestAnimationFrame`, fija una funcion para ser ejecutada mas tarde, pero en lugar de llamarla en el siguiente redibujado
espera un numero de milisegundos  

```js
document.body.style.background = "blue";
setTimeout(function() {
  document.body.style.background = "yellow";
}, 2000);
```

`clearTimeout` cancela la ejecucion de la funcion planeada

```js
var bombTimer = setTimeout(function() {
  console.log("BOOM!");
}, 500);

if (Math.random() < 0.5) { // 50% chance
  console.log("Defused.");
  clearTimeout(bombTimer);
}
```

`setInterval` y `clearInterval` son para lo mismo pero la ejecucion se repite
cada X milisegundos

```js
var ticks = 0;
var clock = setInterval(function() {
  console.log("tick", ticks++);
  if (ticks == 10) {
    clearInterval(clock);
    console.log("stop.");
  }
}, 200);
```

### Debouncing

Hay eventos que se pueden disparar muy muy rapidamente (como `mousemove` o
`scroll` pej). Hay que tener cuidado de no consumir mucho tiempo en su
manejador o la pagina se lageara.  

Se puede usar setTimeout para suavizar el evento y que no haya lageos  

```js
// Aqui se responde a mousemove pero una vez cada 250 milisegundos
function displayCoords(event) {
  document.body.textContent = "Mouse " + event.pageX + ", " + event.pageY;
}
var scheduled = false, lastEvent;
addEventListener("mousemove", function(event) {
  lastEvent = event;
  if (!scheduled) {
    scheduled = true;
    setTimeout(function() {
      scheduled = false;
      displayCoords(lastEvent);
    }, 250);
  }
});
```

---

## AJAX

### Conceptos

> Ajax permite pedir datos al servidor y cargarlos sin tener que refrescar la
> pagina entera  
> Los servidores usan para enviar los datos HTML, XML o JSON  
> Ajax usa un modelo asicrono, permite hacer cosas mientras el navegador espera
> los datos del servidor para cargarlos  

> ![js2](/z-static/images/js/ajax/ajaxFlow.png)


> 1. Peticion: El navegador pide datos al servidor, la peticion puede incluir
> datos que el servidor necesite al igual que un formulario puede enviar datos
> a un servidor  

> 2. En el servidor ocurre lo que sea y se genera una respuesta que puede ser
> en forma de HTML u otro formato como XML o JSON que luego el navegador
> convertira en HTML   

> 3. Respuesta: cuando el navegador recibe la respuesta del servidor dispara un
> evento el cual puede ser usado para ejecutar una funcion javascipt que
> procesara los datos y los mostrara en pantalla  

### XMLHttpRequest Object

> Los navegadores usan el objeto `XMLHttpRequest` para manejar peticiones Ajax.
> Cuando el servidor responde a la peticion del navegador el mismo objeto
> `XMLHttpRequest` procesa el resultado  

* **Peticiones**

> ![js2](/z-static/images/js/ajax/ajaxRequest.png)

> 1. Se instancia el objeto `XMLHttpRequest` para crear un nuevo objeto `xhr`  
> 2. `.open()` - Metodo HTTP , url que manejara la peticion, true|false
> si es asincrono
> 3. `.send()` - Envia la peticion al servidor, se puede pasar informacion
> extra o no

* **Respuestas**

> ![js2](/z-static/images/js/ajax/ajaxResponse.png)

> 1. Cuando el navegador recibe y carga la respuesta del servidor se dispara
> el evento `onload`. Esto provoca que se ejecute una funcion  
> 2. La funcion chequea la propiedad `status` del objeto para asegurarse de
> que la respuesta del servidor esta bien

### Formatos de datos

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>JavaScript AJAX - Loading HTML, JSON y XML</title>
  </head>
  <body>
    <header><h1>THE MAKER BUS</h1></header>
    <h2>The bus stops here.</h2>
    <section id="content"></section>
    <script src="js/data.js"></script>

  </body>
</html>
```

```html
// data.html
<div class="event">
  <img src="img/map-ca.png" alt="Map of California" />
  <p><b>Los Angeles, CA</b><br>
  May 1</p>
</div>
<div class="event">
  <img src="img/map-tx.png" alt="Map of MI casa" />
  <p><b>Austin, TX</b><br>
  May 15</p>
</div>
<div class="event">
  <img src="img/map-ny.png" alt="Map of New York" />
  <p><b>New York, NY</b><br>
  May 30</p>
</div>

```

```json
// data.json
{
  "events": [
    {
      "location": "San Francisco, CA",
      "date": "May 1",
      "map": "img/map-ca.png"
    },
    {
      "location": "Austin, TX",
      "date": "May 15",
      "map": "img/map-tx.png"
    },
    {
      "location": "New York, NY",
      "date": "May 30",
      "map": "img/map-ny.png"
    }
  ]
}
```

```xml
// data.xml
<?xml version="1.0" encoding="utf-8" ?>
<events>
  <event>
    <location>San Francisco, CA</location>
    <date>May 1</date>
    <map>img/map-ca.png</map>
  </event>
  <event>
    <location>Austin, TX</location>
    <date>May 15</date>
    <map>img/map-tx.png</map>
  </event>
  <event>
    <location>New York, NY</location>
    <date>May 30</date>
    <map>img/map-ny.png</map>
  </event>
</events>
```

* **[HTML](//brusbilis.com/chuletas/frontend/html5/)**

```js
var xhr = new XMLHttpRequest();       // Create XMLHttpRequest object

xhr.onload = function() {             // When response has loaded
  if(xhr.status === 200) {            // If server status was ok update
    var response = xhr.responsetext;  
    document.getElementById('content').innerHTML = response;
  }
};

xhr.open('GET', 'data/data.html', true);        // Prepare the request
xhr.send(null);
```

* **[JSON](#json)**

```js
var xhr = new XMLHttpRequest();         // Create XMLHttpRequest object

xhr.onload = function() {               // When readystate changes
  if(xhr.status === 200) {              // If server status was ok
    responseObject = JSON.parse(xhr.responseText);

    // BUILD UP STRING WITH NEW CONTENT (could also use DOM manipulation)
    var newContent = '';
    // Loop through object
    for (var i = 0; i < responseObject.events.length; i++) {
      newContent += '<div class="event">';
      newContent += '<img src="' + responseObject.events[i].map + '" ';
      newContent += 'alt="' + responseObject.events[i].location + '" />';
      newContent += '<p><b>' + responseObject.events[i].location +
                                                        '</b><br>';
      newContent += responseObject.events[i].date + '</p>';
      newContent += '</div>';
    }

    // Update the page with the new content
    document.getElementById('content').innerHTML = newContent;

  //}
};

xhr.open('GET', 'data/data.json', true);        // Prepare the request
xhr.send(null);  
```

* **[XML](#xml)**

```js
var xhr = new XMLHttpRequest();          // Create XMLHttpRequest object

xhr.onload = function() {                // When response has loaded
  if (xhr.status === 200) {              // If server status was ok
    var response = xhr.responseXML;      // Get XML from the server
    // Find <event> elements and loop through them
    var events = response.getElementsByTagName('event');
    for (var i = 0; i < events.length; i++) {            
      var container, image, location, city, newline;     
      container = document.createElement('div');          
      container.className = 'event';                     
      // Add map image
      image = document.createElement('img');              
      image.setAttribute('src', getNodeValue(events[i], 'map'));
      image.setAttribute('alt', getNodeValue(events[i], 'location'));
      container.appendChild(image);
      // Add location data
      location = document.createElement('p');             
      city = document.createElement('b');
      newline = document.createElement('br');
      city.appendChild(document.createTextNode(getNodeValue(
                                            events[i], 'location')));
      location.appendChild(newline);
      location.insertBefore(city, newline);
      location.appendChild(document.createTextNode(getNodeValue(
                                            events[i], 'date')));
      container.appendChild(location);

      document.getElementById('content').appendChild(container);
    }
}
  // Gets content from XML
  function getNodeValue(obj, tag) {                   
    return obj.getElementsByTagName(tag)[0].firstChild.nodeValue;
  }
};

xhr.open('GET', 'data/data.xml', true);             // Prepare the request
xhr.send(null);
```

### Datos de otros servidores

> Por seguridad AJAX no carga respùestas de otros dominios (conocidas como
> `cross-domain requests`)  
> Alternativas

> 1. `Proxy` - Crear un archivo en mi servidor que recoge los datos del
> servidor  remoto. Asi las demas paginas de mi web pueden pedir los datos
> de ese archivo que actua de proxy     
> 2. `JSONP`    
> 3. `Cross-Origin resources Sharing CORS` - Añadir informacion extra a las
> cabeceras HTTP - [Buena Guia para CORS](http://www.html5rocks.com/en/tutorials/cors/?redirect_from_locale=es)

* **JSONP**  

```html
// data-jsonp.html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>JavaScript Ajax - JSONP</title>
</head>
<body>
  <header><h1>THE MAKER BUS</h1></header>
  <h2>The bus stops here</h2>
  <section id="content"></section>
  <script src="js/data-jsonp.js"></script>
  <script
    src="http://htmlandcssbook.com/js/jsonp.js?callback=showEvents">
  </script>
</body>
</html>
```

```js
// js/data-jsonp.js
function showEvents(data) {           // Callback when JSON loads
  var newContent = '';                // Variable to hold HTML
  // BUILD UP STRING WITH NEW CONTENT (could also use DOM manipulation)
  for (var i = 0; i < data.events.length; i++) {    // Loop through object
    newContent += '<div class="event">';
    newContent += '<img src="' + data.events[i].map + '" ';
    newContent += 'alt="' + data.events[i].location + '" />';
    newContent += '<p><b>' + data.events[i].location + '</b><br>';
    newContent += data.events[i].date + '</p>';
    newContent += '</div>';
  }
  // Update the page with the new content
  document.getElementById('content').innerHTML = newContent;
}
```

```js
// data/data-jsonp.js
showEvents({
  "events": [
    {
      "location": "San Francisco, CA",
      "date": "May 1",
      "map": "img/map-ca.png"
    },
    {
      "location": "Austin, TX",
      "date": "May 15",
      "map": "img/map-tx.png"
    },
    {
      "location": "New York, NY",
      "date": "May 30",
      "map": "img/map-ny.png"
    }
  ]
});
```

---

## JSON

### Notacion JSON

JSON es solo texto plano que envias por internet y luego el navegador
convierte en objetos para usarlos  

> ![js2](/z-static/images/js/jsonSyntaxis.png)

> * KEYS colocados entre comillas (no comillas simples y separados del valor
> por dos puntos.

> * VALUES alguno de los siguientes tipos de datos
>     * `string` - Texto escrito entre comillas  
>     * `number` - Numero  
>     * `boolean` - `true` o `false`  
>     * `array` - array de valores o de objetos   
>     * `object` - objeto javascript que puede contener objetos hijos o arrays  
>     * `null` - cuando el valor esta vacio o perdido  

> * Cada pareja clave/valor esta separada por una coma excepto la ultima  

### Objeto JSON

> events es un array que contiene dos objetos (uno por evento)  

```json
{
  "events" : [
    {
      "location": "San Francisco, CA",
      "date": "May l",
      "map": "img/map-ca.png "
    },
    {    
      "location" : "Austin, TX",
      "date" : "May 15",
      "map": "img/map-tx.png"
    }
  ]
}
```

`JSON.stringify()` convierte objetos javascript en una cadena formateada usando
JSON. Esto permite enviar objetos javascript de el navegador a otra aplicacion  


`JSON.parse()` convierte una cadena de datos JSON en objetos javascript que el
navegador pueda usar  

---

## XML

Parecido a HTML pero las etiquetas describen el tipo de dato que hay dentro  
Se puede procesar XML usando los mismos metodos DOM de HTML, por eso es mas
facil usando jQuery  

```xml
<?xml version="1.O" encoding="utf-8" ?>
<events>
  <event>
  <location>San Francisco, CA</location>
  <date>May 1 </date>
  <map>img/map-ca.png</map>
  </event>
  <event>
  <location>Austin, TX</location>
  <date>May 15</ date>
  <map>img/map-tx.png</map>
  </event>
</events>
```

---

## FORMULARIOS

### Helper functions

* Añadir un `event listener`

```js
function addEvent (el, event, callback) {
  if ('addEventListener' in el) {                  
    el.addEventListener(event, callback, false);   
  } else {                                         
    el['e' + event + callback] = callback;         
    el[event + callback] = function () {
      el['e' + event + callback](window.event);
    };
    el.attachEvent('on' + event, el[event + callback]);
  }
}
```

* Borrar un `event listener`

```js
function removeEvent(el, event, callback) {
  if ('removeEventListener' in el) {                       
    el.removeEventListener(event, callback, false);        
  } else {                                                
    el.detachEvent('on' + event, el[event + callback]);   
    el[event + callback] = null;
    el['e' + event + callback] = null;
  }
}
```

### Elemento Form

El `document` tiene una coleccion de formularios que guarda referencias a
cada formulario de la pagina

> * `document.forms[1]` accede al segundo formulario de la pagina  
> * `document.forms.login` accede al formulario cuyo atributo name="login"

Cada elemento `<form>` de la pagina tiene tambien una coleccion de elementos
que tiene todos los controles del formulario dentro

> * `document.forms[1].element[0]` accede al primer control del segundo
> formulario de la pagina
> * `documento.forms[1].elements.password` accede al elemento del segundo
> formulario cuyo atributo name="password"   

* **Propiedades**

`action` - La URL a que el formulario es enviado para ser procesado  
`method` - si se envia por GET o POST  
`name` - se usa poco, lo mas comun es seleccionar un formulario por su
atributo id  
`elements` - una coleccion de elementos en el formulario con los que el
usuario puede interactuar. Se puede acceder a ellos por los indices o por
los valores de sus atributos ´name´  

* **Metodos**

`submit()` - Tiene el mismo efecto que pinchar el boton submit   
`reset()` - Resetea el formulario a los valores iniciales como si la pagina
se hubiera recargado  

* **Eventos**

`submit` - Se dispara cuando el formulario es enviado  
`reset` - Se dispara cuando se resetea el formulario  

### Controles del formulario

* **Propiedades**

`value` - En una entrada de texto es el texto que el usuario introduce, es
el valor del atributo `value`  
`type` - Cuando un formulario se crea usando el elemento `<input>` esto define
el tipo del elemento (text, radio, checkbox, password ..)  
`name` - Devuelve o establece el valor de atributo `name`  
`defaultValue` - Es el valor inicial de una entrada de texto cuando la pagina
se renderiza  
`form` - El formulario al que el control pertenece  
`disabled` - Inhabilita el elemento del formulario  
`checked` - Indica que checkboxes o botones de radio han sido chequeados.
Es un booleano que indica true si esta chequeado  
`defaultChecked` - El valor inicial de un checkbox o radio button (booleano)  
`selected` - Indica que un elemento de una select box esta seleccionado.
Es un booleano que indica true si esta seleccionado.  

* **Metodos**

`focus()` - Le da el foco a un elemento  
`blur()` - Le quita el fofo al elemento  
`select()` - Selecciona e ilumina el contenido de texto de un elemento de
entrada de texto  
`click()` - Dispara un    

>  * evento `click` en botones, checkboxes y carga de archivos.  
>  * evento `submit` en el boton submit   
>  * evento `reset` en el boton reset  

* **Eventos**

`blur` - Salta cuando el usuario sale de un campo  
`focus` - Salta cuando el usuario entra en un campo  
`click` - Salta cuando el usuario pincha un elemento  
`change` - Salta cuando el valor de un elemento cambia   
`input` - Salta cuando cambia el valor de los elementos `<input>` ó
`<textarea>`   
`keydown`, `keyup`, `keypress` - Salta cuando se interactua con el teclado  

---

## JAVASCRIPT APIs

Para ver si un navegador tiene una funcionalidad

```js
if (navigator.geolocation) {
  // codigo
} else {
  // codigo
}
```

[Modernizr puede ayudar](https://modernizr.com/)

```js
if (Modernizr.awesomeNewFeature) {
  showOffAwesomeNewFeature();
} else {
  getTheOldLameExperience();
}
```

---

## WEB STORAGE

### Objeto Storage

> Los navegadores guardan sobre 8mb por dominio en un objecto storage    
> Los datos se guardan como propiedades (clave/calor) del objeto storage  
> Acceso a los datos se hace de manera sincrona  
> Los navegadores usan politica del mismo origen, vamos que los datos solo son
> accesibles por otras paginas del mismo dominio  
> Para ello las 4 partes del URL deben coincidir

>>>>>>>   ![apis](/z-static/images/apis/url.png)

>>> 1. Protocolo ¡ ojo que https es distinto a http !  
>>> 2. Subdominio, maps.google.com no puede acceder a datos guardados de
>>> www.google.com  
>>> 3. Dominio debe coincidir  
>>> 4. Puerto, este tambien debe coincidr

* **Propiedades**

`.length` - numero de claves  

* **Metodos**

`.setItem('clave', 'valor')` - Crea una nueva pareja clave/valor  
`.getItem('clave')` - Devuelve el valor de la clave "clave"  
`.removeItem('clave')` - Elimina la clave/valor para esa clave  
`.clear()` - Limpia toda la informacion del objeto storage  

>>>>> ![apis](/z-static/images/apis/webStorage1.png)  

* **sessionStorage**

> * Cambios frecuentes cada vez que el usuario visita el sitio, (datos de    localizacion, logueos)  
> * Es personal y privado, no debe ser visto por otros usuarios del dispositivo  

* **localStorage**

> * Solo cambia en los intervalos establecidos (horarios, listas de precios)
> y es util almacenarlo offline  
> * Cosas que el usuario use de nuevo si vuelve al sitio (guardar preferencias
> y ajustes)

```js
localStorage.setItem("username", "marijn");
console.log(localStorage.getItem("username"));
// → marijn
localStorage.removeItem("username");
```

> Un valor en `localStorage` dura hasta que se sobreescribe, se elimina o el
> usuario limpia sus datos locales  
> Cada pagina tiene su propio almacen que solo puede interactuar con scripts de
> la misma pagina   

* **Ejemplo**

```js
var basicRecipes = [{
  title: 'Cookies',
  ingredients: ['Cup shortening', 'Peanut Butter', 'Milk', 'Eggs']
}, {
  title: 'Spaghetti',
  ingredients: ['Noodles', 'Tomato Sauce', 'Meatballs', 'Onion']
}, {
  title: 'Rice Pudding',
  ingredients: ['White Rice', 'Milk', 'Sugar', 'Salt']
}];

// Estas dos hacen lo mismo
localStorage.setItem('basicRecipes', JSON.stringify(basicRecipes));
localStorage.basicRecipes = JSON.stringify(basicRecipes);
// Dos formas de hacer lo mismo tambien
var sol = JSON.parse(localStorage.getItem('basicRecipes'));
console.log(sol[0].title);
console.log((JSON.parse(localStorage.basicRecipes))[0]);
// Esto borra toda la localStorage
localStorage.clear()
```

### IndexedDB

### Web SQL

Deprecada, pero aun se usa  
No funciona ni en chrome ni en IE  

---

## GEOLOCATION

* **Metodos objeto `navigation.geolocation`**

`getCurrentPosition(exito, error, conf)` -
> exito - funcion para procesar la ubicacion recibida en el objeto `Position`
> error -funcion para procesar los errores retornados en el objeto
> `PositionError`  
> conf - objeto para configurar como la informacion sera adquirida  

`watchPosition(exito, error, conf)` - igual que el anterior excepto que
inicia un proceso de vigilancia para detectar nuevas ubicaciones que nos
enviara cada cierto tiempo  

`clearWatch(id)` - El metodo `watchPosition()` retorna un valor que puede ser
almacenado en una variable para luego ser usado como id aqui y detener la
vigilancia  

* **Propiedades objeto `Position`**  

`.coords.latitude` - Devuelve latitud en grados decimales  
`.coords.longitude` - Devuelve longitud en grados decimales  
`.coords.accuracy` - Precision de latitud y longitud en metros  
`.coords.altitude` - Devuelve metros sobre el nivel del mar  
`.coords.altitudeAccuracy` - Precision de la altitud en metros  
`.coords.heading` - Devuelve grados respecto al norte  
`.coords.speed` - Devuelve velocidad en m/s  
`.coords.timestamp` - Devuelve tiempo desde que se creo (en forma de `Date`)  

* **Propiedades objeto `PositionError`**

`PositionError.code` - Devuelve un numero de error con los valores:  
> 1. Permiso denegado  
> 2. No disponeble  
> 3. Ha expirado el tiempo  

`PositionError.message` - Devuelve un mensaje (pero no para el usuario)  

```js
var elMap = document.getElementById('loc');                 
var msg = 'Sorry, we were unable to get your location.';    

if (Modernizr.geolocation) {                                
  navigator.geolocation.getCurrentPosition(success, fail);  
  elMap.textContent = 'Checking location...';               
} else {                                                    
  elMap.textContent = msg;                                  
}
function success(position) {                                
  msg = '<h3>Longitude:<br>';                               
  msg += position.coords.longitude + '</h3>';               
  msg += '<h3>Latitude:<br>';                               
  msg += position.coords.latitude + '</h3>';                
  elMap.innerHTML = msg;                                    
}
function fail(msg) {                                        
  elMap.textContent = msg;                                  
  console.log(msg.code);                                    
}
```

---

## HISTORY

### Objeto history  

* **Propiedades**

`.length` - numero de articulos en el objeto historia    

* **Metodos**

`history.back()` - Retrocedes en la historia a la anterior posicion  
`history.forward()` - Saltas adelante a la siguiente posicion  
`history.go(n)` - Te lleva a la pagina n respecto de tu posicion que es la 0.
Por ejemplo -2 echa dos para atras y 1 salta uno hacia adelante   
`history.pushState()` - Añade un elemento a la historia  
`history.replaceState()` - Cambia el actual elemento de la historia por el que
pasamos ahora  

* **Eventos**

`window.onpopstate` - Usado para manejar los movimientos de usuario de adelante
atras  

---

## GOOGLE MAPS

### API Key

[Se consigue aqui](https://cloud.google.com/console)

### Ajustes

* **Crear un mapa**

> 1. El evento `onload` llama a la funcion `loadScript()`   
> 2. `LoadScript()` crea un elemento `<script>` que carga la API y cuando se carga
> llama a `init()` que inicializa el mapa  
> 3. `init()`  carga el mapa en la pagina. Primero crea un objeto `mapOptions`
> con propiedades  
> 4. Luego usa el contructor Map() para crear el map y dibujarlo en la pagina. El
> contructor tiene dos paremetros  
>     * el elemento dentro del cual el mapa aparecera dentro  
>     * el objeto `mapOption`  

`zoom` - Entre 0 (el mundo entero) y 16  
`mapTypeId` - ROADMAP, SATELLITE, HYBRID, TERRAIN  


```js
function init() {
  var mapOptions = {           // Set up the map options
    center: new google.maps.LatLng(40.782710,-73.965310),
    mapTypeId: google.maps.MapTypeId.ROADMAP,
    zoom: 13
  };
  var venueMap;                // Map() draws a map
  venueMap = new google.maps.Map(document.getElementById('map'),
                                                    mapOptions);
}
function loadScript() {
  var script = document.createElement('script');     
  script.src = 'http://maps.googleapis.com/maps/api/js?sensor=false&
                                                     callback=init';
  document.body.appendChild(script);                 
}
window.onload = loadScript;    
```

* **Cambiar los controles**

>>>>> ![apis](/z-static/images/apis/mapControls.png)

![apis](/z-static/images/apis/controls.png)

```js
function init() {
  var mapOptions = {
    zoom: 14,
    center: new google.maps.LatLng(40.782710,-73.965310),
    mapTypeId: google.maps.MapTypeId.ROADMAP,

    panControl: false,
    zoomControl: true,
    zoomControlOptions: {
      style: google.maps.ZoomControlStyle.SMALL,
      position: google.maps.ControlPosition.TOP_RIGHT
    },
    mapTypeControl: true,
    mapTypeControlOptions: {
      style: google.maps.MapTypeControlStyle.DROPDOWN_MENU,
      position: google.maps.ControlPosition.TOP_LEFT
    },
    scaleControl: true,
    scaleControlOptions: {
      position: google.maps.ControlPosition.TOP_CENTER
    },
    streetViewControl: false,
    overviewMapControl: false
  };
  var venueMap = new google.maps.Map(document.getElementById('map'),
                                                        mapOptions);
}
function loadScript() {
  var script = document.createElement('script');
  script.src = 'http://maps.googleapis.com/maps/api/js?sensor=false
                                                    &callback=init';
  document.body.appendChild(script);
}
window.onload = loadScript;
```

* **Añadir marcadores**

```js
var pinlocation = new google.maps.Latlng(40.782710,-73.965310);
var startPosition = new google.maps.Marker({    // Create marker
  position: pinLocation,                        // Set position
  map: venueMap,                                // Specify the map
  icon: "img/go.png"                            // Path to image
});
```

---

## CANVAS

### Graficos para la web

`<canvas>` - Crea el lienzo para dibujar  

```html
<body>
  <section id="cajacanvas">
    <canvas id="canvas" width="500" height="300"></canvas>
  </section>
</body>
```

`.getContext(opcion)` - Genera un contexto de dibujo que se asigna al lienzo.
opcion puede ser "2d" o "webGL"  

```js
function iniciar(){
  var elem = document.getElementById('lienzo');
  var lienzo = elem.getContext('2d');
  // Aqui los distintos ejemplos posteriores
}
addEventListener("load", iniciar);
```

### Dibujar

>>>>> ![apis](/z-static/images/apis/canvasCoords.png)

```js
function iniciar(){
  var elemento=document.getElementById('lienzo');
  lienzo=elemento.getContext('2d');
  // codigo para dibujar y hacer cosas
}
window.addEventListener("load", iniciar, false)
```

* **Rectangulos**

> Metodos  

`fillRect(x,y,ancho,alto)` - Dibuja un rectangulo solido. La esquina superior
izquierda esta en x,y. Ancho y alto definen el tamaño del rectangulo  
`strokeRect(x,y,ancho,alto)` - Como el anterior pero solo dibuja el contorno  
`clearRect(x,y,ancho,alto)` - Es un borrador rectangular  

```js
lienzo.strokeRect(100,100,120,120);
lienzo.fillRect(110,110,100,100);
lienzo.clearRect(120,120,80,80);
```

* **Color**

> Propiedades

`strokeStyle` - color para el contorno de la figura  
`fillStyle` - color para el interior de la figura  
`globalAlpha` - especifica la transfercnia para todas las figuras dibujadas
en el lienzo  

```js
lienzo.fillStyle = "#000099";
lienzo.strokeStyle = "#990000";
lienzo.strokeStyrl = "rgba(255, 165, 0, 1)"
lienzo.globalAlpha = 0.5                     // (0 opaco, 1 transparente)
```

* **Degradados**

> Metodos

`createLinearGradient(x1,y1,x2,y2)` - Crea un objeto que luego sera usado
para aplicar un gradiente lineal al lienzo   
`createRadialGradient(x1,y1,r1,x2,y2,r2)` - Crea un objeto que luego será
usado para aplicar un gradiente circular o radial al lienzo usando dos
círculos. Los valores representan la posición del centro de cada círculo y
sus radios  
`addColorStop(posicion,color)` - Posición es un valor entre 0 y 1 que
determina dónde la degradación comenzará para ese color en particular.
Color especifica los colores que usaran los gradientes  

```js
var gradiente = lienzo.createLinearGradient(0, 0, 10, 100);
gradiente.addColorStop(0.5, '#0000FF');
gradiente.addColorStop(1, '#000000');
lienzo.fillStyle = gradiente;
```

* **Crear Trazados**

Lo normal es procesar figuras en segundo plano y una vez hecho enviarlas al
contexto a ser dibujadas.

Un `trazado` es como un mapa a ser seguido por el lapiz. Puede incluir
diferentes tipos de líneas, como líneas rectas, arcos, rectángulos ... para
crear figuras complejas  

> Metodos para comenzar y cerrar el trazado

`beginPath()` - Describe el comienzo de una nueva figura. Se llama primero,
antes de comenzar a crear el trazado  
`closePath()` - Cierra el trazado generando una linea recta desde el ultimo
punto hasta el punto de origen. Se puede ignorar cuando usamos el metodo
`fill()` para dibujar el trazado en el lienzo  

> Metodos para dibujar el trazado en el lienzo

`stroke()` - dibuja el trazado de una figura vacia (solo el contorno)  
`fill()` - dibuja el trazado de una figura solida  
`clip()` - declara una nueva area de corte para el contexto. Al inicializar
el contexto el area de corte es el area completa ocupada por el lienzo.
`clip()` cambia esa area a una nueva forma creando una mascara. Todo lo que
este fuera de esa mascara no sera dibujado  

```js
lienzo.beginPath();
// aquí va el trazado
lienzo.stroke();
```

> Metodos para crear el trazado

`moveTo(x,y)` - mueve el lapiz a una posicion para continuar con el trazado  
`lineTo(x,y)` - genera linea recta desde la posicion actual hasta la nueva x,y  
`rect(x,y,ancho,alto)` - genera un texangulo que forma parte del trazado  
`arc(x,y,radio,anguloInicio,anguloFinal,direccion)` - genera un arco o circulo
en la posicion x,y con radio y desde un anguloInicio hasta anguloFinal. La
direccion false a favor de las agujas del reloj, true en contra  
`quadraticCurve(cpx,cpy,x,y)` - genera una curva cuadratica bezier desde la
posicion actual hasta las posicion x,y. cpx y cpy indican el punto que dara
forma a la curva  
`bezierCurve(cp1x,cp1y,cp2x,cp2y,x,y)` - como el anterior pero genera una curva bezier cubica con dos puntos para moldear la curva  

```js
lienzo.beginPath();
lienzo.moveTo(100,100);
lienzo.lineTo(200,200);
lienzo.lineTo(100,200);
// Opcion 1
lienzo.closePath();     lienzo.stroke();
// Opcion 2
lienzo.fill();
```

```js
// circulos con arc()
lienzo.arc(100,100,50,0,Math.PI*2, false);
// arco de 45 grados
var radianes=Math.PI/180*45;
lienzo.arc(100,100,50,0,radianes, false);
```

* **Estilos de linea**

> Propiedades afectan al trazado completo. para cambia las caracteristicas de
> las lineas hay que crear un nuevo trazado  

`lineWidth` - Determina el grosor de la linea, por defecto = 1  
`lineCap` - Determina la forma de la terminacion de la linea `butt`,
`round` ó `square`  
`lineJoin` - Forma de la conexion entre dos lineas, `round`, `bevel`
ó `miter`  
`miterLimit` - Determina cuanto la conexion de dos lineas sera extendida
cuando lineJoin="miter"   

```js
lienzo.beginPath();
lienzo.arc(200,150,50,0,Math.PI*2, false);
lienzo.stroke();

lienzo.lineWidth=10;
lienzo.lineCap="round";
lienzo.beginPath();
lienzo.moveTo(230,150);
lienzo.arc(200,150,30,0,Math.PI, false);
lienzo.stroke();

lienzo.lineWidth=5;
lienzo.lineJoin="miter";
lienzo.beginPath();
lienzo.moveTo(195,135);
lienzo.lineTo(215,155);
lienzo.lineTo(195,155);
lienzo.stroke();
```

* **Texto**

> Propiedades

`font` - similar a `font` de CSS y acepta los mismos valores  
`textAlign` - Alinea el texto, `start`, `end`, `left`, `right`, y `center`  
`textBaseline` -Alineamiento vertical, `top`, `hanging`, `middle`,
`alphabetic`, `ideographic`, y `bottom`  

> Metodos

`strokeText(texto,x,y,opcional)` - Dibuja el texto en la posicion x,y como una figura vacia(solo contornos). opcional declara el tamaño maximo,si el texto es
mas extenso se encogera    
`fillText(texto,x,y)` - Igual que el anterior pero el texto sera solido  
`measureText(texto,x,y)` - Retorna informacion sobre el tamaño de un texto
especifico. Util para combinar texto con otras formas y calcular posiciones o
colisiones  

```js
lienzo.font="bold 24px verdana, sans-serif";
lienzo.textAlign="start";
lienzo.fillText("Mi mensaje", 100,100);
```

```js
lienzo.font="bold 24px verdana, sans-serif";
lienzo.textAlign="start";
lienzo.textBaseline="bottom";
lienzo.fillText("Mi mensaje", 100,124);

var tamano=lienzo.measureText("Mi mensaje");
lienzo.strokeRect(100,100,tamano.width,24);
```

* **Sombras**

> Propiedades

`shadowColor` - Color de la sombra usando sintaxis CSS  
`shadowOffsetX` - Recibe un numero que indica cuan lejos esta la sombra del
objeto en direccion horizontal  
`shadowOffsetY` - Recibe un numero que indica cuan lejos esta la sombra del
objeto en direccion vertical  
`shadowBlur` - Produce efecto de difuminacion para la sombra  


```js
lienzo.shadowColor="rgba(0,0,0,0.5)";
lienzo.shadowOffsetX=4;
lienzo.shadowOffsetY=4;
lienzo.shadowBlur=5;
lienzo.font="bold 50px verdana, sans-serif";
lienzo.fillText("Mi mensaje ", 100,100);
```

* **Transformaciones**

`translate(x,y)` - Mueve el origen del lienzo  
`rotate(angulo)` - Rota el lienzo alrededor del origen tantos angulos  
`scale(x,y)` - Incrementa o disminuye las unidades de la grilla para reducir
o ampliar todo lo dibujado. La escala se puede cambiar solo en un eje.
Por defecto valor=1  
`transform(m1,m2,m3,m4,dx,dy)` - El lienzo tiene una matriz de valores, esto
aplica una nueva matriz sobre la actual para modificar el lienzo  
`setTransform(m1,m2,m3,m4,dx,dy)` - Reinicializa la matriz de transformacion
y establece una nueva con estos valores  

```js
// Moviendo, rotando y escalando.
lienzo.fillText("PRUEBA",50,20);
lienzo.translate(50,70);
lienzo.rotate(Math.PI/180*45);
lienzo.fillText("PRUEBA",0,0);
lienzo.rotate(-Math.PI/180*45);
lienzo.translate(0,100);
lienzo.scale(2,2);
lienzo.fillText("PRUEBA",0,0);
```

```js
// Transformaciones acumulativas sobre la matriz.
lienzo.transform(3,0,0,1,0,0);
lienzo.font="bold 20px verdana, sans-serif";
lienzo.fillText("PRUEBA",20,20);
lienzo.transform(1,0,0,10,0,0);
lienzo.font="bold 20px verdana, sans-serif";
lienzo.fillText("PRUEBA",100,20);
```

* **Restaurar el estado**

> Metodos

`save()` - graba es estado del lienzo  
`restore()` - recupera el ultimo estado grabado  

```js
// Grabando el estado del lienzo.
lienzo.save();
lienzo.translate(50,70);
lienzo.font="bold 20px verdana, sans-serif";
lienzo.fillText("PRUEBA1",0,30);
lienzo.restore();
lienzo.fillText("PRUEBA2",0,30);
```

* **globalCompositeOperation**

> Determina como una figura es poscionada y combinada con figuras ya dibujadas
> en el lienzo  
> Valores de la propiedad  

`source-over` - POR DEFECTO - la nueva figura sera dibujada sobre las
existentes  
`source-in` - Solo la parte de la nueva figura que se sobrepone a las figuras previas es dibujada. El resto de la figura, e incluso el resto de las figuras previas, se vuelven transparentes.  
`source-out` - Solo la parte de la nueva figura que no se sobrepone a las
figuras previas es dibujada. El resto de la figura, e incluso el resto de las figuras previas, se vuelven transparentes.  
`source-atop` - Solo la parte de la nueva figura que se superpone con las
figuras previas es dibujada. Las figuras previas son preservadas, pero el
resto de la nueva figura se vuelve transparente.  
`lighter` - Ambas figuras son dibujadas (nueva y vieja), pero el color de las partes que se superponen es obtenido adicionando los valores de los colores de cada figura.  
`xor` - Ambas figuras son dibujadas (nueva y vieja), pero las partes que se superponen se vuelven transparentes.  
`destination-over` - Este es el opuesto del valor por defecto. Las nuevas
figuras son dibujadas detrás de las viejas que ya se encuentran en el lienzo.  
`destination-in` - Las partes de las figuras existentes en el lienzo que se superponen con la nueva figura son preservadas. El resto, incluyendo la nueva figura, se vuelven transparentes  
`destination-out` - Las partes de las figuras existentes en el lienzo que no
se superponen con la nueva figura son preservadas. El resto, incluyendo la
nueva figura, se vuelven transparentes.  
`destination-atop` - Las figuras existentes y la nueva son preservadas solo en la parte en la que se superponen.  
`darker` - Ambas figuras son dibujadas, pero el color de las partes que se superponen es determinado substrayendo los valores de los colores de cada
figura.  
`copy` - Solo la nueva figura es dibujada. Las ya existentes se vuelven transparentes.  

```js
// robando la propiedad globalCompositeOperation
lienzo.fillStyle="#990000";
lienzo.fillRect(100,100,300,100);
lienzo.globalCompositeOperation="destination-atop";
lienzo.fillStyle="#AAAAFF";
lienzo.font="bold 80px verdana, sans-serif";
lienzo.textAlign="center";
lienzo.textBaseline="middle";
lienzo.fillText("PRUEBA",250,110);
```

### Procesar imagenes

* **drawImage()**

> Permite dibujar una imagen en el lienzo

`drawImage(imagen,x,y)` - Dibuja una imagen en el lienzo en la posicion x,y.
La imagen puede ser una referencia a un elemento `<img>` `<video>` u otro
`<canvas>`  
`drawImage(imagen,x,y,ancho,alto)` - Como antes pero permite escalar la imagen  
`drawImage(imagen,x1,y1,ancho1,alto1,x2,y2,ancho2,alto2)` - Los valores ..1
definen la parte de la imagen que sera cortada mientras que los valores ..2
indican el lugar donde sera insertado en el lienzo y su nuevo tamaño  

```js
var img = document.createElement('img');
img.setAttribute('src', 'http://www.formasterminds.com/snow.jpg');
img.addEventListener("load", function(){
  lienzo.drawImage(img, 20, 20);
  // Otra opcion: Ajustando la imagen al tamaño del lienzo
  lienzo.drawImage(img, 0, 0, elem.width, elem.height);
});
```

* **Datos de imagen**

> Toda imagen puede ser representada por una sucesión de números enteros
> representando valores rgba  Un grupo de valores con esta información
> resultará en un array unidimensional que puede ser usado luego para generar
> una imagen.

> Metodos

`getImageData(x,y,ancho,alto)` - toma un rectangulo del lienzo y lo convierte
en datos. Retorna un objeto con propiedades `width`, `height` y `data`   
`putImageData(datosImagen,x,y)` - convierte los datos de `datosImagen` en una
imagen y la dibujan en el lienzo en la posicion x,y  
`createImageData((ancho,alto)|datos)` - Crea datos para representar una imagen vacia.
Todos los pixeles son negro transparente. Tambien puede recibir datos como
atributo en lugar de ancho,alto  

```js
img = document.createElement('img');
img.setAttribute('src', 'snow.jpg');
img.addEventListener("load", modimagen);

function modimagen(){
  lienzo.drawImage(img, 0, 0);

  var info = lienzo.getImageData(0, 0, 175, 262);
  var pos;
  for(var x = 0; x < 175; x++){
    for(var y = 0; y < 262; y++){
      pos = (info.width * 4 * y) + (x * 4);
      info.data[pos] = 255 - info.data[pos];
      info.data[pos+1] = 255 - info.data[pos+1];
      info.data[pos+2] = 255 - info.data[pos+2];
    }
  }
  lienzo.putImageData(info, 0, 0);
}
```

* **cross-origin**

`setAttribute("crossOrigin","anonymous|use-credentials")` - `anonymous` hace
caso omiso de las credenciales y `use-credentials` requiere credenciales  

```js
imagen.setAttribute("crossOrigin","anonymous|use-credentials");
```

* **Extraccion de datos**

> Metodos

`toDataURL(image/jpeg|image/png)` - Devuelve los datos en formato data:url del
contenido del lienzo a una resolucion de 96 ppp   
`toDataURL` - Como la anterior pera la resolucion es la original del lienzo  
`toBlob(funcion,image/jpeg|image/png)` -  Devuelve un objeto con un
blob(datos en crudo) que contiene la representacion del lienzo en el formato elegido y resolucion de 96 ppp. La funcion es la encargada de procesar
el objeto.  
`toBlobHD(tipo)` - Como el anterior pero la resolucion del blob es la del
lienzo original  

* **Patrones**

> Los patrones son simples adiciones que pueden mejorar nuestros trazados.  

`createPattern(imagen,tipo)` - imágen es una referencia a la imagen que vamos
a usar como patrón, y tipo configura el patrón por medio de cuatro
valores: repeat, repeat-x, repeat-y y no-repeat  

```js
img = document.createElement('img');
  img.setAttribute('src', 'http://www.formasterminds.com/content/bricks.jpg');
  img.addEventListener("load", modimagen);
}
function modimagen(){
  var pattern = lienzo.createPattern(img, 'repeat');
  lienzo.fillStyle = pattern;
  lienzo.fillRect(0, 0, 500, 300);
}
```

### Animaciones

> Básicamente, debemos borrar el área del lienzo que queremos animar, dibujar
> las figuras y repetir el proceso una y otra vez.  
> Es mejor usar imagenes (png) que figuras con trazados complejos


* **Elementales**

```js
// Dos ojos que miran al puntero del raton y lo siguen
var lienzo;
function iniciar(){
  var elem = document.getElementById('lienzo');
  lienzo = elem.getContext('2d');
  addEventListener('mousemove', animar);
}
function animar(e){
  lienzo.clearRect(0, 0, 300, 500);

  var xraton = e.clientX;
  var yraton = e.clientY;
  var xcentro = 220;
  var ycentro = 150;
  var ang = Math.atan2(xraton - xcentro, yraton - ycentro);
  var x = xcentro + Math.round(Math.sin(ang) * 10);
  var y = ycentro + Math.round(Math.cos(ang) * 10);

  lienzo.beginPath();
  lienzo.arc(xcentro, ycentro, 20, 0, Math.PI * 2, false);
  lienzo.moveTo(xcentro + 70, 150);
  lienzo.arc(xcentro + 50, 150, 20, 0, Math.PI * 2, false);
  lienzo.stroke();

  lienzo.beginPath();
  lienzo.moveTo(x + 10, y);
  lienzo.arc(x, y, 10, 0, Math.PI * 2, false);
  lienzo.moveTo(x + 60, y);
  lienzo.arc(x + 50, y, 10, 0, Math.PI * 2, false);
  lienzo.fill();
}
addEventListener("load", iniciar);
```

* **Profesionales**

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <title>VideoJuegos</title>
  <style>
    body{
      text-align: center;
    }
    #cajadelienzo{
      margin: 100px auto;
    }
    #lienzo{
      border: 1px solid #999999;
    }
  </style>
  <script src="videojuego.js"></script>
</head>
<body>
  <section id="cajadelienzo">
    <canvas id="lienzo" width="600" height="400"></canvas>
  </section>
</body>
</html>
```

`requestAnimationFrame(funcion)` - Sincroniza la animacion con la ventana del
navegador y el monitor. hay que llamarlo para cada ciclo del bucle  
`cancelAnimationFrame(variable)` - Podemos asignar el valor de arriba a una variable y con ese metodo cancelamos el proceso  

Hay que concentrar el codigo del juego en un objeto global unico

```js
var onoff;
function gameLoop() {
  // haces lo que sea  
  onoff = requestAnimationFrame(gameLoop);
}
// en otro sitio para parar el bucle
cancelAnimationFrame(onoff);
```

```js
var mijuego = {
  lienzo: {
    ctx: '',
    offsetx: 0,
    offsety: 0
  },
  nave: {
    x: 300,
    y: 200,
    movex: 0,
    movey: 0,
    speed: 1
  },
  iniciar: function(){
    var elem = document.getElementById('lienzo');
    mijuego.lienzo.ctx = elem.getContext('2d');
    mijuego.lienzo.offsetx = elem.offsetLeft;
    mijuego.lienzo.offsety = elem.offsetTop;
    document.addEventListener('click', function(e){
      mijuego.control(e);
    });
    mijuego.bucle();
  },
  bucle: function(){
    if(mijuego.nave.speed){
      mijuego.procesar();
      mijuego.detectar();
      mijuego.dibujar();
      // requestAnimationFrame(function(){ mijuego.bucle() });
      webkitRequestAnimationFrame(function(){ mijuego.bucle() });
    }else{
      mijuego.lienzo.ctx.font = "bold 36px verdana, sans-serif";
      mijuego.lienzo.ctx.fillText('JUEGO FINALIZADO', 112, 210);
    }
  },
  control: function(e){
    var distancex = e.clientX - (mijuego.lienzo.offsetx + mijuego.nave.x);
    var distancey = e.clientY - (mijuego.lienzo.offsety + mijuego.nave.y);
    var ang = Math.atan2(distancex, distancey);
    mijuego.nave.movex = Math.sin(ang);
    mijuego.nave.movey = Math.cos(ang);
    mijuego.nave.speed += 1;
  },
  dibujar: function(){
    mijuego.lienzo.ctx.clearRect(0, 0, 600, 400);
    mijuego.lienzo.ctx.beginPath();
    mijuego.lienzo.ctx.arc(mijuego.nave.x, mijuego.nave.y, 20, 0,
                                            Math.PI/180*360, false);
    mijuego.lienzo.ctx.fill();
  },
  procesar: function(){
    mijuego.nave.x += mijuego.nave.movex * mijuego.nave.speed;
    mijuego.nave.y += mijuego.nave.movey * mijuego.nave.speed;
  },
  detectar: function(){
    if(mijuego.nave.x < 0 || mijuego.nave.x > 600 || mijuego.nave.y < 0
                                              || mijuego.nave.y > 400){
      mijuego.nave.speed = 0;
    }
  }
};
addEventListener('load', function(){ mijuego.iniciar(); });
```

### Procesar Video

> Coger cada cuadro del video desde el elemento `<video>` y dibujarlo como
> una imagen en el lienzo usando drawImage()  

* **Mostrar video**

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <title>Video en el Lienzo</title>
  <style>
    section{
      float: left;
    }
  </style>
  <script>
    var lienzo, video;
    function iniciar(){
      var elem = document.getElementById('lienzo');
      lienzo = elem.getContext('2d');
      video = document.getElementById('media');
      lienzo.translate(483, 0);
      lienzo.scale(-1, 1);
      setInterval(procesarCuadros, 33);
    }
    function procesarCuadros(){
      lienzs.drawImage(video, 0, 0);
    }
    addEventListener("load", iniciar);
  </script>
</head>
<body>
  <section>
    <video id="medio" width="483" height="272" autoplay>
      <source src="http://www.formasterminds.com/content/trailer2.mp4">
      <source src="http://www.formasterminds.com/content/trailer2.ogg">
    </video>
  </section>
  <section>
    <canvas id="canvas" width="483" height="272"></canvas>
  </section>
</body>
</html>
```

### Graficos de tarta

>> ![apis](/z-static/images/apis/pieChart.png)

```html
<!doctype html>
<canvas width="600" height="300"></canvas>
<script>
var results = [
  {name: "Contento", count: 1043, color: "lightblue"},
  {name: "Neutral", count: 563, color: "lightgreen"},
  {name: "Descontento", count: 510, color: "pink"},
  {name: "No contesta", count: 175, color: "silver"}
];
var cx = document.querySelector("canvas").getContext("2d");
var total = results.reduce(function(sum, choice) {
  return sum + choice.count;
}, 0);

var currentAngle = -0.5 * Math.PI;
var centerX = 300, centerY = 150;

results.forEach(function(result) {
  var sliceAngle = (result.count / total) * 2 * Math.PI;
  cx.beginPath();
  cx.arc(centerX, centerY, 100,
         currentAngle, currentAngle + sliceAngle);

  var middleAngle = currentAngle + 0.5 * sliceAngle;
  var textX = Math.cos(middleAngle) * 120 + centerX;
  var textY = Math.sin(middleAngle) * 120 + centerY;
  cx.textBaseLine = "middle";
  if (Math.cos(middleAngle) > 0)
    cx.textAlign = "left";
  else
    cx.textAlign = "right";
  cx.font = "15px sans-serif";
  cx.fillStyle = "black";
  cx.fillText(result.name, textX, textY);

  currentAngle += sliceAngle;
  cx.lineTo(centerX, centerY);
  cx.fillStyle = result.color;
  cx.fill();
});
</script>
```

---

## FILE

![apis](/z-static/images/comingSoon2.jpg)

---

## POUCH DB

[PouchDB](https://pouchdb.com/)

---

## REDIS

[Redis](http://redis.io/)

---
