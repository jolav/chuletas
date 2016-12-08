# JAVASCRIPT

`console.log()`<br>
`prompt("introduce datos aqui")`<br>
`confirm("Aceptas si o no")`<br>
`alert("Ventana en la pantalla")`

`//` Comentarios de una linea<br>
`/* ... /*` Comentarios multilinea

--------------------------------------------------------------------------------

## OPERADORES

- **Aritmeticos**

> `+` Suma<br>
> `-` Resta<br>
> `*` Multiplicacion<br>
> `/` Division<br>
> `%` Modulo, lo que sobra de la division entera<br>
> `++` Incremento<br>
> `--` Decremento

- **Asignacion**

> `=` x = y<br>
> `+=` x = x + y<br>
> `-=` x = x - y<br>
> `*=` x = x * y<br>
> `/=` x = x / y<br>
> `%=` x = x % y

- **Comparacion**

> `==` igual<br>
> `===` igual valor e igual tipo<br>
> `!=` no igual<br>
> `!==` no igual valor ni igual tipo<br>
> `>` mayor que<br>
> `<` menor que<br>
> `<=` mayor o igual que<br>
> `>=` menor o igual que

- **Logicos**

> `&&` AND<br>
> `||` OR<br>
> `!` NOT

- **Unitarios**

> `typeof()` - Nos da el tipo de valor

> `in` - Devuelve true si la propiedad especificada existe en el objeto especificado

> > `propiedadNombreOrNumero in nombreObjeto`<br>
> > `propiedadNombreOrNumero` es una string que representa al nombre de una propiedad o un numero que es el indice de un array<br>
> > `nombreObjeto` es el nombre de un objeto

```javascript
var trees = new Array("redwood", "bay", "cedar", "oak", "maple");
0 in trees;        // returns true
3 in trees;        // returns true
6 in trees;        // returns false
"bay" in trees;    // returns false (you must specify the index number,
                   // not the value at that index)
"length" in trees; // returns true (length is an Array property)
```

--------------------------------------------------------------------------------

## VARIABLES

En javascript las variables no tienen tipos, son los valores quienes tienen tipos

- Las variables se pasan siempre por valor, pero cuando una variable se refiere a un objeto (incluyendo arrays) ese valor es una referencia al objeto  
- Asi cambiar el valor de la variable no cambia el objeto o valor primitivo subyacente, solo apunta la variable a una nueva primitiva u objeto  
- Al cambiar una propiedad de un objeto referenciado por una variable cambia el objeto que subyacente    


### Alcance

La visibilidad es hacia fuera en las funciones. Yo veo las variables de las funciones externas que me contienen pero no veo las de dentro

**Global**

- Declarada fuera de una funcion

```javascript
var carName = 'volvo';  
// codigo aqui puede usar carName  
function myFunction() {
  // codigo aqui puede usar carName
}
```

- Si asigno un valor a una variable no declarada, esta se convierte en global

```javascript
// codigo aqui puede usar carName
function myFunction() {
  carName = 'volvo';
  // codigo aqui puede usar carName
}
```

**Local**

Variables declaradas dentro de una funcion permanecen locales a esa funcion

```javascript
// codigo aqui NO puede usar carName
function myFunction() {
  var carName = 'volvo';
  // codigo aqui puede usar carName
}
```

### Conversion de tipos

`Number(string)` - Convierte cadena string en un numero<br>
`parseInt(string)` - Convierte la cadena string en un numero entero<br>
`parseFloat(string)` - Convierte la cadena string en un numero flotante<br>
`String(numero)` - Convierte el numero en una string<br>
`numero.toString()` - equivale a String(numero) y devuelve una string

--------------------------------------------------------------------------------

## DATOS BASICOS

`typeof dato`<br>
Devuelve una string con el tipo de la variable dato

![js1](/z-static/images/js/tiposDatos.png)

### Numeros

> - Internamente son un numero flotante de 64 bits

>   - `NaN (Not a number)` no equivale a ningun valor ni siquiera a el mismo

- Metodos :

`isNan(numero)` - Detecta si es numero o no<br>
`numero.toFixed(n)` - Redondea a n decimales (devuelve una string)<br>
`numero.toPrecision(n)` - Redondea a n digitos decimales incluidos (devuelve una string)<br>
`numero.toExponential(n)` - Representa el numero en notacion exponencial (devuelve una string)<br>
`numero.toString()`- equivale a `String(numero)` y devuelve una string

#### Math

`Math` es un Objeto Global que actua sobre numeros

- Propiedades

`Math.PI` - Devuelve pi 3.14159265359

- Metodos

`Math.round(num)` - Redondea num al entero mas cercano<br>
`Math.sqrt(num)` - Devuelve la raiz cuadrada del numero positivo num<br>
`Math.ceil(num)` - Redondea al entero mas cercano por arriba<br>
`Math.floor(num)` - Redondea al entero mas cercano por abajo<br>
`Math.random()` - Genera un numero aleatorio entre 0 (inclusivo) y 1 (no inclusivo)

```javascript
// Crea un numero aleatorio entre 1-10
var aleatorio = Math.floor((Math.random() * 10) +1);
```

### String

> - Puede estar entre comillas simples o comillas dobles
> - Las cadenas tienen un propiedad length. `"cadena".length`
> - Las cadenas son inmutables. no se pueden cambiar pero si crear nuevas
> - Para escapar caracteres usamos \ `\n` nueva linea por ejemplo

- Metodos

`parseInt(string)` - Convierte la cadena string en un numero<br>
`String.fromCharCode()` - Convierte un codigo de letra en una cadena con la letra

```javascript
var codigo = 65;
var letra = String.fromCharCode(codigo);      // letra = "A"
```

`string.charAt(pos)` - Devuelve la letra en la posicion pos

```javascript
var name = 'Curly';
var initial = name.charAt(0);       // initial is 'C'
```

`string.charCodeAt(pos)` - Devuelve el codigo de la letra en la posicion pos

```javascript
var name = 'Curly';
var initial = name.charCodeAt(0);   // initial is 67
```

`string.concat(str1, str2, ...)` - Junta los strings. Mejor usar +

```javascript
var s = 'C'.concat('a', 't');       // s es 'Cat'
```

`string.indexOf(cadenaBuscada, pos)` - Busca una cadenaBuscada en la string y si la encuentra devuelve la posicion del primer caracter, si no devuelve -1.<br>
El parametro opcional pos indica a partir de donde buscar

```javascript
var text = 'Mississippi';
var p = text.indexOf('ss');         // p es 2
p = text.indexOf('ss', 3);          // p es 5
p = text.indexOf('ss', 6);          // p es -1
```

`string.lastIndexOf(cadenaBuscada, pos)` - Como indexOf pero busca desde atras hacia adelante

```javascript
var text = 'Mississippi';
var p = text.indexOf('ss');         // p es 5
p = text.indexOf('ss', 3);          // p es 2
p = text.indexOf('ss', 6);          // p es 5
```

`string.replace(valorBuscado, valorNuevo)` - Reemplaza el valorBuscado por el valorNuevo solo una vez salvo que el valorBuscado sea una expresion regular

```javascript
var str = "Visita mi casa";
var res = str.replace("casa", "huerta"); // "Visita mi huerta"
```

`string.search(regexp)` - Busca una cadena que case con la expresion regular pasada y devuelve la posicion del primer caracter o -1

```javascript
var text = 'Por la mañana " yo me levanto';
var pos = text.search(/["']/);          // pos es 14
```

`string.slice(comiezo, fin)` - Crea una nueva string desde la posicion comienzo incluida hasta la posicion fin NO incluida

```javascript
var cadena = 'Hola mundo';
var res = cadena.slice(1, 4);           // res es 'ola'
```

`string.split(separador, limite)` - Crea un array de cadenas al separar el string original en piezas separadas por el parametro separador<br>
Limite es el limite de nuevas cadenas

```javascript
var cadena = "Como lo llevas hoy amigo";
var res = cadena.split(" ", 4)  // res es ['Como','lo','llevas','hoy']
```

`string.substring(comienzo, fin)` - Usar `string.slice()` en su lugar<br>
`string.toLowerCase()` - Convierte la string a minusculas<br>
`string.toUpperCase()` - Convierte la string a mayusculas<br>
`string.fromCharCode(car1, car2, ...)`

```javascript
var a = String.fromCharCode(67, 97, 116);
// a is 'Cat'
```

`string.trim(cadena)` - Elimina los espacios en blanco y los saltos de linea del comienzo y del final de la cadena

### Boolean

`true` o `false`

Operadores Logicos

```javascript
AND --> &&
OR  --> ||
NOT --> !
Operador ternario o operador condicional
test ? expresion1 : expresion2
Si test es true devuelve expresion1, si es falso devuelve expresion2
```

Cuando convertimos un no-booleano a booleano es falso o true?

```javascript
-- Falso
"" cadena Vacia
0, -0, NaN
null, undefined
false
-- Todo lo demas es true
```

### Null - Undefined

Se usan para denotar la ausencia de valor<br>
Muchas operaciones en JS que no producen un valor significativo dan undefined sencillamente porque un valor tienen que dar

`null` es para objetos<br>
`undefined` es para propiedades, metodos o variables

--------------------------------------------------------------------------------

## ESTRUCTURAS DE CONTROL

### if ... else

```html
if (condicion) {
  instrucciones_if_condicion_es_true;
else {
  instrucciones_if_condicion_es_false;
}
```

En cascada

```javascript
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

```javascript
while (condicion) {
  instrucciones_while_condicion_es_true;
}
```

`do while` El codigo se ejecuta minimo una vez

```javascript
do {
  instrucciones_while_condicion_es_true
while (condicion)
```

### for

`inicializacion` - Se ejecuta antes que el bucle empiece<br>
`condicion` - Define la condicion para seguir ejecutando el bucle<br>
`actualizacion` - Se ejecuta cada vez que el bucle se ha ejecutado

```html
for (inicializacion; condicion; actualizacion) {
  codigo_se_ejecuta:con_cada_bucle;
}

for (var i = 0; i < 10; i++) {
  document.writeln(i);
}
```

### for in

Recorre todas las propiedades de un objeto o array

```javascript
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

```javascript
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

```javascript
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

Los arrays en Javascript son falsos arrays pero aun asi renta su uso<br>
Heredan de `Array.prototype` muchos metodos muy utiles

- Creacion de Arrays

> - Array Literal

```javascript
var vacio = [];
var numeros = ['cero','uno','dos','tres','cuatro','cinco',
               'seis','siete','ocho','nueve','diez'];
vacio[1]    // undefined
numeros[1]  // 'one'
```

> - Array constructor

```javascript
var colores = new Array('blanco', 'rojo', 'azul')
```

- Iteracion de arrays

```javascript
No usar for in que da muchos problemas por no ser objetos puros
for ( var i = 0; i < numeros.length; i = i + 1) {
  console.log(numeros[i]);
}
```

- Lazy initialization

En arrays multidimensionales, el segundo array no se crea por defecto y hay que inicializarlo antes de usarlo

```javascript
var matriz = [];
for (var numFila = 0; numFila < filas; numFila++) {
  // lazy initialization
  if (!matriz[numFila]) { matriz[numFila] = []; }
  for (var numColumna = 0; numColumna < cols; numColumna++) {
    matriz[numFila][numColumna] = data[indice + numFila][numColumna];
  }
}
```

```javascript
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

- **Añadir elementos**

`array.push(item1, item2, ...)` - Añade los items al final del array. Pero cada item que añade lo hace como un array item

```javascript
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.push(b, true);
// a is['a', 'b', 'c', ['x', 'y', 'z'], true]
// c is 5
```

`array.unshift(item1, item2, ...)` - como push pero añade los items al principio del array

```javascript
var a = ['a', 'b', 'c'];
var r = a.unshift('?', '@');
// a es ['?', '@', 'a', 'b', 'c']
//r es 5
```

- **Eliminar elementos**

`array.pop()` - Elimina y devuelve el ultimo elemento del array. Si vacio devuelve undefined

```javascript
var a = ['a', 'b', 'c'];
var c = a.pop( ); // a is ['a', 'b'] & c is 'c'
```

`array.shift()` - elimina el primer elemento del array y lo devuelve Si el array esta vacio devuelve undefined

```javascript
var a = ['a', 'b', 'c'];
var c = a.shift( );
// a es ['b', 'c'] , c is 'a'
```

- **Iteracion**

`forEach(valor, indice)` - Ejecuta una funcion una vez para cada elemento del array.  

```javascript
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

`some()` - Comprueba si algunos elementos del array pasan un test definido por una funcion

```javascript
var ages = [3, 10, 18, 20];
function checkAdult(age) {
    return age >= 18;
}
function myFunction() {
    res = ages.some(checkAdult);
}
// res = true
```

`every()` - Comprueba si todos los elementos del array pasan un test definido por una funcion

```javascript
var ages = [3, 10, 18, 20];
function checkAdult(age) {
    return age >= 18;
}
function myFunction() {
    res = ages.every(checkAdult);
}
// res = false
```

- **Combinar**

`array.concat(item1, item2 ...)` - Va añadiendo los items

```javascript
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.concat(b, true);
// c is ['a', 'b', 'c', 'x', 'y', 'z', true]
```

- **Filtrar**

`filter()` - Crea un nuevo array con todos los elementos que pasan un test especificado en una funcion

```javascript
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

```javascript
result = result.filter(function (val, i, res) {
  console.log(val, i, res);
});
```

`reduce()` - Reduce el array a un solo valor. Ejecuta una funcion suministrada para cada valor del array(de izda a derecha). El valor de return se guarda en un acumulador

```javascript
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

- **Buscar**

`array.indexOf("elemento")` - Busca el elemento en el array y devuelve su posicion o -1 si no lo encuentra  

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple");
// a = 2
```  

`array.includes(value)` - Devuelve false or true si encuentra o no el elemento dentro del array  

```javascript
function disemvowel (str) {
  var vowels = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'];
  var res = [];
  str.split('').forEach(function (value, index) {
    if (!vowels.includes(value)) res.push(value)
    //if (vowels.indexOf(value) === -1) res.push(value);
  });
  return res.join('');
}
```

- **Ordenar**

`array.sort(funcionDeComparacion)` - Por defecto la comparacion la hace asumiendo que todos los elementos son strings

```javascript
var n = [4, 8, 15, 16, 23, 42];
n.sort(function (a, b) {
  return a - b;
});
// n is [4, 8, 15, 16, 23, 42];
```

`array.reverse()` - invierte todos los elementos del array

```javascript
var a = ['a', 'b', 'c'];
var b = a.reverse( );
// tanto a como b son ['c', 'b', 'a']
```

- **Modificar**

`map()` - Llama una funcion sobre cada elemento del array y crea un nuevo array con los resultados

```javascript
var numbers = [4, 9, 16, 25];

function myFunction() {
  x = numbers.map(Math.sqrt);
// x = [2, 3, 4, 5]
```

`array.join(separador)` - crea un string concatenando todos los elementos del array usando el separador indicado que por defecto es ','. Si usas espacio en blanco como separador se unen todos sin separacion

```javascript
var a = ['a', 'b', 'c'];
a.push('d');
var c = a.join(''); // c is 'abcd';
```

`array.slice(comienzo, fin)` - copia una parte del array desde el array[comienzo] incluido hasta el array[fin] NO incluido. Fin es opcional y por defecto es array.length

```javascript
var a = ['a', 'b', 'c'];
var b = a.slice(0, 1);  // b es ['a']
var c = a.slice(1); // c es ['b', 'c']
var d = a.slice(1, 2); // d es ['b']
```

`array.splice(comienzo, borrarCont, item1, item2, ...)` - Elimina elementos reemplazandolos por los nuevos item. array[comienzo] a partir de aqui (el incluido) elimina los siguientes borrarCont e inserta los items en su lugar Devuelve los elementos borrados

```javascript
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

- Las funciones son enlazadas a `Function.prototype` que a su vez esta enlazado a `Object.prototype`
- Cada funcion se crea con una propiedad prototype cuyo valor es un objeto con una propiedad constructor cuyo valor es la funcion
- Cada funcion se crea con dos propiedades ocultas

  - El contexto de la funcion
  - El codigo que implementa el comportamiento de la funcion

- Para evitar callback hell y mas faunas mejor usar siempre las funciones todas con nombre

### Tipos de funciones

- **Named functions**

> - Declaracion de funciones (function declaration) :las puedes llamar cuando quieras antes de que se lean, incluso en el onload

```javascript
function nombreFuncion () {
  codigo;
}
```

> - Expresiones de funciones (function expressions) : no se pueden usar hasta que este encontrada y definida

```javascript
ANONIMAS
var nombreFuncion = function() {
  codigo;
}
NAMED
var referenciaFuncion = function nombreFuncion() {
  codigo;
}
```

- **Anonymous functions**

```javascript
function () {
  codigo;
}
```

- **Autoejecutables**

Immediately Invoked Function Expressions (IIFEs)<br>
Una vez declarada se llama a si misma para inicializarse y estar ya disponible para otras partes de la aplicacion<br>
Se usa para declarar variables que no afectan al resto del codigo fuera de la funcion pues contiene la visibilidad de las variables<br>
Pueden usar tambien `return`

```javascript
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

```javascript
NAMED
(function nombreFuncion(){
    //código de tu función
}());
```

### Invocacion

- Se realiza `nombrefuncion();`

Cuando invocamos una funcion

```javascript
1- Se suspende la ejecucion de la funcion actual
2- Pasa el control y los parametros a la nueva funcion
3- Tambien se pasan this y arguments
4- El valor de this depende del patron de invocacion de los cuales
existen 4
```

Patrones de invocacion

```javascript
- Metodos, this se refiere al objecto desde el que se invoca el metodo
- Funcion, this se refiere al objeto global
- Constructor, this se refiere al nuevo objeto que se esta construyendo
- Apply PENDIENTE
```

### Parametros y Argumentos

Pequeña diferencia entre ambos

- Parametros: son las variables que se definen cuando se declara la funcion
- Argumentos: son los valores que se pasan a la funcion al invocarla
- Si a una funcion le pasamos demasiados parametros los que sobran los ignora
- Si a una funcion le pasamos menos parametros a los que faltan les asigna undefined

`arguments object` - Un objeto similar a un Array que se corresponde con los argumentos pasados a la función.

```javascript
function fourArguments (a, b, c) {
  console.log(arguments.length);
  console.log(arguments[0]);
  console.log(arguments[3]);
}
fourArguments('uno', 'dos', 'tres', 'cuatro');
```

### return multiples valores

Para ello usan un array

```javascript
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

Es una forma de "recordar" y tener acceso a las variables de una funcion una vez que ya ha terminado de ejecutarse.

Closure ejemplo1

```javascript
function crearContador(x) {
  function sumarCantidad(y) {
    return x + y;
  }
  return sumarCantidad
}
var sumarUno = crearContador(1);
```

Closure ejemplo2

```javascript
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

```javascript
function nombreCompleto (nombre, apellido, callback) {
  console.log("Me llamo " + nombre + " " + apellido);
  callback(apellido);
}
var saludos = function (ape) {
  console.log("Hola, Sr " + ape);
};
nombreCompleto("Brus", "Bilis", saludos);
```

```javascript
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

```javascript
// url.js
db.getList(printList);

function printList (rows) {
  console.log('rows', rows);
}
// database.js
getList: function (callback) {
  console.log('list');
  con.query('SELECT * FROM url', function (err, rows) {
    if (err) throw err;
    callback(rows);
  });
},
```

### Recursividad

Una funcion que se llama a si misma es recursiva

```javascript
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

Un Objeto es un contenedor de propiedades<br>
Una propiedad es un nombre y un valor (cualquiera menos undefined)<br>
Los objetos se pasan siempre por `referencia`

### Crear Objetos

- **Literales**

  - Los valores de las propiedades se pueden obtener de cualquier expresion incluso de otros objetos anidados
  - Bueno para la creación de nuevos valores de objeto
  - NO son reusables

```javascript
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

- **Constructor**

```javascript
// NORMAL

var persona = new Object();
persona.nombre = 'Ivan';
persona.edad = '50';
```

```javascript
// REUSABLE

function myPersona (a, b) {
  this.name = a;
  this.edad = b;
}
var actor1 = new myPersona ('Juan', '30');
var actor2 = new myPersona ('Manu', '18');
```

### this

Usada dentro de funciones y objetos para referirse a un objeto que lo normal es el objeto en el que la funcion opera

- Funcion con alcance global (no esta dentro de ningun otro objeto o funcion) `this` se refiere al objeto `window`

```javascript
function windowSize() {
  var width = this.innerWidth;
  var height = this.innerHeight;
  return [height, width]
}
```

- Variables globales, las variables globales se convierten en propiedades del objeto `window`

```javascript
var width = 600;
var shape = {width: 300};
var showWidth = function() {
  document.write(this.width);
};
showWidth();      // this.width = 600
```

- Metodo de un objeto, si una funcion se define dentro de un objeto se convierte en un metodo de ese objeto. En un metodo `this` se refiere al objeto que lo contiene

```javascript
var shape = {
  width: 600,
  height: 400,
  getArea: function() {
    return this.width * this.height;
  }  
}
```

- Funciones con nombre globales y usadas como metodos de un objeto, `this` se refiere al objeto que contiene a la funcion

```javascript
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

```javascript
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

- `for in` itera sobre todas las propiedades del objeto incluso funciones o propiedades de los prototipos
- Para evitarlo se usan filtros como `typeof !== 'function'` ó `hasOwnProperty`
- Tambien puede usar el `for`

### Metodos

`object.hasOwnProperty(nombre)` Devuelve true si el objeto tiene la propiedad nombre y false si no. El prototipo no se examina

```javascript
var a = {member: true};
var b = Object.create(a);
var t = a.hasOwnProperty('member'); // t is true
var u = b.hasOwnProperty('member'); // u is false
var v = b.member                    // v is true
```

### Herencia

- En javascript los objetos heredan directamente de otros objetos (prototipos)
- Con el operador `new` podemos montarnos factorias de objetos y hacer un pseudoclasico patron de diseño

--------------------------------------------------------------------------------

## ERRORES

### Contexto de ejecucion

> Se corresponden con el alcance o visibilidad de las variables

> - `Contexto Global`: codigo que esta en el script pero no en una funcion. Solo hay un contexto global en una pagina
> - `Contexto de funcion`: codigo que se ejecuta dentro de una funcion. Cada funcion tiene su propio contexto

> Cada vez que un script entra en un nuevo contexto de ejecucion hay dos fases :

> - `Preparacion`:, se crea un nuevo alcance. Se crean variables, funciones y argumentos y se determina el valor de `this`.
> - `Ejecucion`: ahora se pueden asignar valores a las variables, se referencian las funciones y se ejecuta su codigo y se ejecutan tambien las sentencias

- **Objeto variables**

> - Cada contexto de ejecucion tiene su propio objeto `variables` que tiene las variables, funciones y parametros que estan disponibles para ese contexto.
> - Cada contxto de ejecucion tiene tambien acceso al padre de objeto `variables`

- **Excepciones**

Cuando se produce un error, mira en su contexto a ver si hay codigo para manejar el error, si no lo hay sube hacia arriba por el stack buscando codigo para manejar el error. Si llega al contexto global y no lo encuentra termina la ejecucion del script y crea un objeto `Error`

### Objecto Error

- **Tipos de objetos error**

`Error` - error generico `SyntaxError` - no se ha respetado la sintaxis<br>
`ReferenceError` - se ha referenciado una variable que o no esta declarada o esta fuera del alcance<br>
`TypeError` - hay un inesperado tipo de datos que no puede ser forzado<br>
`RangeError` - numeros en un rango no aceptable +0 `URIError` - metodos del tipo encodeURI() decodeURI() mal usados<br>
`EvalError` - funcion eval() mal usada

- **Propiedades**

`name` - tipo de ejecucion<br>
`message` - descripcion<br>
`fileNumber` - nombre del archivo javascript<br>
`lineNumber` - numero de la linea con error

### Depuracion

> - **¿ Donde esta el fallo ?**

1. Hay que acotar al maximo el area donde esta el problema.

2. El mensaje de error te dice:

  - el script donde esta el problema
  - el numero de linea donde el script ya no puede continuar. Lo normal es que el fallo este antes
  - el tipo de error

3. Prueba a ver hasta donde se ejecuta el script uando mensajes en la consola para ello
4. Usa breakpoints para parar la ejecucion e inspeccionar los valores almacenados en las variables

> - **¿ Cual es el fallo ?**

1. En los breakpoints si ves valores normales busca antes
2. Parte el codigo en partes mas pequeñas para testearlas

  - usa la consola para escribir los valores de las variables
  - llama a las funciones desde la consola para ver si devuelven lo que se espera de ellas
  - comprueba si los objetos existen y tienen los metodos y propiedades que se espera que tengan

3. Comprueba el numero de parametros de una funcion o el numero de elementos de un array.

> - **Trucos**

- Prueba otro navegador, a veces los errores son especificos del navegador
- Manda numeros a la consola a ver cuando se para la ejecucion del programa
- Comenta partes del codigo para ir acotando zonas
- StackOverflow es tu amigo
- Herramientas de validacion de codigo online:

  - [jslint](http://jslint.com)
  - [jshint](http://jshint.com)
  - [jsonlint](http://jsonlint.com)

### Manejo excepciones

Interrumpen la ejecucion del programa. Para evitarlo hay que capturarlas

```javascript
try {
  // intenta ejecutar este codigo
} catch (exception) {
  // si hay una excepcion ejecuta este codigo
} finally {
  // esto siempre se ejecuta
}
```

```javascript
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

Si sabes que algo podria causar un fallo puedes generar tus propios errores antes que el interprete lo haga.

`throw new Error("mensaje");`

```javascript
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

```javascript
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

--------------------------------------------------------------------------------

## STRICT MODE

Se activa poniendo `"use strict"` al comienzo del archivo o del cuerpo de una funcion

- No permite usar variables no declaradas
- No se puede borrar una variable u objeto usando `delete`
- No se puede definir la misma propiedad dos veces
- No permite los nombres de parametros duplicados
- En una funcion si no se conoce `this` en lugar de usar el objeto global `window` sera `undefined`
- Las variables instanciadas dentro del contexto de eval() sólo son válidas en ese contexto.
- La sentencia `with(){}` ya no se usa
- Y mas ...

**js en cliente**

Envolvemos todo la parte del codigo js en una IIFEs

```javascript
(function() { 'use strict'; /* code here */

.. codigo

}());
```

**node.js**

añadimos al comienzo del archivo

```javascript
/*jslint node: true */
'use strict';
```

--------------------------------------------------------------------------------

## MODULOS

[Modulos](/backend/nodejs/#modulos)

--------------------------------------------------------------------------------

## PATRONES

### Reducir variables globales

`funciones autoejecutables IIFEs`

```javascript
(function () {
  // todas las variables y funciones son locales a esto
  // pero aun tiene acceso a todas las globales
}());
```

`Importacion global`

```javascript
(function ($, YAHOO) {
    // Nos da acceso a las globales jQuery (as $) y YAHOO aqui dentro
}(jQuery, YAHOO));
```

`var MiAPP = {}` y la convierto en el contenedor de todo para mi aplicacion.

```javascript
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

### Objetos IIFE y mezclas

```javascript
var app = (function () {
  'use strict';

  function init () {
    console.log('Inicio', Dungeon.hola());
    renderer.setMyCanvas();
    renderer.drawMyCanvas();
  }

  return {
    init: init
  };
}());

var renderer = (function () {
  'use strict';
  var cols = 80;
  var rows = 50;
  var ppp = 10;

  function setMyCanvas () {
    canvas = document.getElementById('myCanvas');
    ctx = canvas.getContext('2d');
    canvas.width = cols * ppp;
    canvas.height = rows * ppp;
  }

  function drawMyCanvas () {
    console.log('Drawing Map...');
  }
  return {
    setMyCanvas: setMyCanvas,
    drawMyCanvas: drawMyCanvas
  };
}());

var Dungeon = {
  hola: function () {
    return 'ROGUE';
  }
};

addEventListener('load', app.init);
```

### Datos: Arrays VS Objects

> Grupos de obejtos se pueden almacenar en arrays o como propiedades de otros objetos  

- **Objetos en un array**

```javascript
var people = [
  {name: 'Casey ', rate: 70, active: true},
  {name: 'Camille', rate: 80, active: true},  
  {name: 'Gordon', rate: 75, active: false},
  {name: 'Nigel', rate: 120, active: true}
]
```

- **Objetos como propiedades de otros objetos**

```javascript
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

- **const**

Crea constantes que no se pueden modificar, ha de asignarse el valor en la declaracion

- **let**

Declara variables que no son accesibles mas alla del ambito de creacion<br>
Ejemplo mejor: para los `for`

### funcion arrow

Para las funciones anonimas

```javascript
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

```javascript
let nombre1 = "JavaScript";  
let nombre2 = "awesome";  
console.log(`Sólo quiero decir que ${nombre1} is ${nombre2}`);
```

### modulos

bar.js

```javascript
function hello(who) {
    return "Let me introduce: " + who;
}
export hello;
```

foo.js

```javascript
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

```javascript
module foo from "foo";      // import the entire "foo" and "bar" modules
module bar from "bar";
console.log(
    bar.hello( "rhino" )
);                             // Let me introduce: rhino
foo.awesome();                 // LET ME INTRODUCE: HIPPO
```

--------------------------------------------------------------------------------

## PROMISES

```javascript
var p = new Promise(function(resolve, reject) {  
   if (/* condition */) {
      resolve(/* value */);  // fulfilled successfully
   }
   else {
      reject(/* reason */);  // error, rejected
   }
});
```

```javascript
(function () {
  'use strict';
  /* code here */

  // Get a reference to the body element, and create a new image object
  var body = document.querySelector('body');
  var myImage = new Image();
  // Call the function with the URL we want to load, but then chain the
  // promise then() method on to the end of it. This contains two callbacks
  imgLoad('darth.jpg').then(function (response) {
    // The first runs when the promise resolves, with the request.reponse
    // specified within the resolve() method.
    var imageURL = window.URL.createObjectURL(response);
    myImage.src = imageURL;
    body.appendChild(myImage);
  // The second runs when the promise
  // is rejected, and logs the Error specified with the reject() method.
  }, function (Error) {
    console.log(Error);
  });
/* otra sintaxis
  }).catch(function (Error) {
    console.log(Error);
  });
*/

  function imgLoad (url) {
    // Create new promise with the Promise() constructor
    // This has as its argument a function
    // with two parameters, resolve and reject
    return new Promise(function (resolve, reject) {
      // Standard XHR to load an image
      var request = new XMLHttpRequest();
      request.open('GET', url);
      request.responseType = 'blob';
      // When the request loads, check whether it was successful
      request.onload = function () {
        if (request.status === 200) {
          // If successful, resolve the promise by passing back the request response
          resolve(request.response);
        } else {
          // If it fails, reject the promise with a error message
          reject(Error("Image didn't load successfully; error code:" +
            request.statusText));
        }
      };
      request.onerror = function () {
        // Also deal with the case when the entire request fails to begin     with
        // This is probably a network error, so reject the promise with an appropriate message
        reject(Error('There was a network error.'));
      };
      // Send the request
      request.send();
    });
  }
}());
```   

---
