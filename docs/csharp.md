# C\# (CSHARP)

---

`//` Comentarios de una linea    
`/* ... /*` Comentarios multilinea

[Array](https://docs.microsoft.com/en-us/dotnet/api/system.array?view=netcore-3.0)  
[Dictionary](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=netcore-3.0)   
[List](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=netcore-3.0)  
[Archivos System.IO.File](https://docs.microsoft.com/en-us/dotnet/api/system.io.file?view=netcore-3.0)

---

## OPERADORES


- **Aritmeticos**

> `+` Suma    
> `-` Resta    
> `*` Multiplicacion    
> `/` Division    
> `%` Modulo, lo que sobra de la division entera    
> `++` Incremento    
> `--` Decremento

```csharp
int i = 3;
Console.WriteLine(i);   // output: 3
Console.WriteLine(i++); // output: 3
Console.WriteLine(i);   // output: 4

double a = 1.5;
Console.WriteLine(a);   // output: 1.5
Console.WriteLine(++a); // output: 2.5
Console.WriteLine(a);   // output: 2.5
```

- **Comparacion**

> `==` igual valor   
> `!=` no igual    
> `>` mayor que    
> `<` menor que    
> `<=` mayor o igual que    
> `>=` menor o igual que

- **Logicos**

> `&&` AND    
> `||` OR    
> `!` NOT

* **Preferencia**  

1) `-` unario minus, aplicar el menos a los numeros negativos  
2) `*` multiplicacion    
3) `/` division   
4) `+` suma  
5) `-` resta      

---

## VARIABLES

* **Declaracion**

```csharp
// tipo nombreVariable
string nombre, otroNombre;
int cantidad;
string hi = Console.ReadLine();

// constantes, hay que asignarles valor al declararlas
const int miNumero = 7;
miNumero = 10; // error
```

* **Using declaracion** una declaracion de variable precedida por `using` avisa al compilador de que debe liberarla al final del bloque que incluye la declaracion  

```csharp
using var myRes = new MyResource();
myRes.DoSomething();
```


* **Var**

Usando var el tipo de variable lo elige el compilador segun el valor que asignamos a la variable

```csharp
var name = "Pedro";
var age = 18;

Console.WriteLine(name.GetType()); // System.String
Console.WriteLine(age.GetType()); // System.Int32
```

---

### Tipos Por Valor

Contienen el valor de variable.

* **Enteros**

| tipo | tamaño | rango |
| :---: | :---: | :---: |
| **sbyte** | 8 bits | -128 a 127 |
| **byte** | 8 bits | 0 a 255 |
| **short** | 16 bits | -32.768 a 32.767 |
| **ushort** | 16 bits | 0 a 65.535 |
| **int** | 32 bits | -2.147.483.648 a 2.147.483.647 |
| **uint** | 32 bits | 0 a 4.294.967.295 |
| **long** | 64 bits | -9.223.372.036.854.775.808 a 9.223.372.036.854.775.807 |
| **ulong** | 64 bits | 0 a 18.446.744.073.709.551.615 |

* **Reales**

| tipo | tamaño | precision | rango |
| :---: | :---: | :---: | :---: |
| **float** | 32 bits | 7 digitos | 1,5E-45 to 3,4E48 |
| **double** | 64 bit | 15 digitos | 5,0E-324 to 1,7E308 |
| **decimal** | 128 bit | 28-29 digitos | 5,0E-324 to 1,7E308 |
|  |  |  |  |

* **Booleanos**

| tipo | tamaño | rango |
| :---: | :---: | :---: |
| **bool** |  | true o false |
|  |  |  |

* **Char**

| tipo | tamaño | rango |
| :---: | :---: | :---: |
| **char** | 16 bits | 0 a 65.535 |
|  |  |  |

```csharp
// se declara usando comillas simples
char caracter = 'a';
```

### Tipos Por Referencia

No contienen el valor sino la direccion de memoria donde esta el valor de la variable

* **Object**  

Convertir un tipo por valor a objecto se llama `boxing`    
Convertir un objeto a un tipo de valor es `unboxing`  

```csharp
object obj;
obj = "Hola"; // boxing
```

* **Dynamic**  

La comprobacion del tipo de valor de estas variables se hace en ejecucion en lugar de durante la compilacion  

```csharp
dynamic <nombreVariable> = valor;
```

* **String**

Para escapar caracteres usamos \ `\n` nueva linea por ejemplo   
Se declaran usando comillas dobles  

```csharp
String str = "Una cadena de texto";
```

Si usamos la @ antes de la cadena significa que interprete la cadena literalmente (sin escapar caracteres), util para rutas de archivos o webs

```charp
String rutaAlArchivo = @"/home/usuario/loQueSea/miArchivo.txt";
```

```csharp
// Cadena a entero
if (miCadena.IsInt()) {
  miEntero=miCadena.AsInt();
}

// Cadena a real
if (miCadena.IsFloat()) {
  miReal=miCadena.AsFloat();
}

// Cadena a decimal
if (miCadena.IsDecimal()) {
  miDecimal=miCadena.AsDecimal();
}

// Cadena a Tipo Fecha
miCadena="10/10/2012";
if (miCadena.isDateTime() {
  miFecha=miCadena.AsDateTime();
}

// Cadena a Booleano
miCadena="True";
if (miCadena.IsBool()) {
  myBool=miCadena.AsBool();
}

// Cualquier dato a cadena
miEntero=1234;
miCadena=miEntero.ToString();
```

### Conversion de tipos

* **Implicita**

Se hacen automaticamente convertiendo un tipo pequeño a uno mayor o una clase derivada a una clase base  
char -> int -> long -> float -> double

```csharp
int miEntero = 9;
double miReal = miEntero;       
`//` Comentarios de una linea    
`/* ... /*` Comentarios multilinea

Console.WriteLine(miEntero);      // 9
Console.WriteLine(miReal);        // 9

```

* **Explicita**

Se hace manualmente

1) Colocando el tipo deseado en parentesis delante del valor a convertir  

```csharp
double miReal = 1.11;
int miEntero = (int) miReal;  

Console.WriteLine(myReal);   // 1.11
Console.WriteLine(myEntero); // 1

```

2) Usando metodos integrados como `Convert` s

```csharp
int miEntero = 10;
double miReal = 5.25;
bool miBooleano = true;

Console.WriteLine(Convert.ToString(miEntero));   // 10
Console.WriteLine(Convert.ToDouble(miEntero));   // 10   
Console.WriteLine(Convert.ToInt32(miReal));      // 5
Console.WriteLine(Convert.ToString(miBooleano)); // True
```

---

## ESTRUCTURAS DE CONTROL  

### **if**

```csharp
if (condicion1) {
  esto se ejecuta si la condicion1 es true
} else if (condicion2) {
  esto se ejecuta si condicion1 es false y condicion2 es true
} else {
  esto se ejecuta si condicion1 es false y condicion2 es false
}
```

- **Ternario**  

`condición ? true : false`  

```csharp
int edad = 18;
string mayorEdad = edad >= 18 ? "Eres mayor" : "Eres menor";
Console.WriteLine(mayorEdad);
```

### for

`inicializacion` - Se ejecuta antes que el bucle empiece    
`condicion` - Define la condicion para seguir ejecutando el bucle    
`actualizacion` - Se ejecuta cada vez que el bucle se ha ejecutado

```csharp
for (inicializacion; condicion; actualizacion) {
  codigo_se_ejecuta_con_cada_bucle;
}

for (int i = 0; i < 10; i++) {
  Console.WriteLine(i);
}
```

### foreach

Es como el `for` pero para iterar sobre elementos en un array  

```csharp
foreach (type nombreVariable in nombreArray) {
    codigo_se_ejecuta_con_cada_bucle;
}

string[] coches = {"Peugeot", "Opel", "Seat", "Mercedes"};
foreach (string i in coches) {
  Console.WriteLine(i);
}
```

### while

`while` El codigo puede que no se ejecute nunca

```csharp
while (condicion) {
  instrucciones_while_condicion_es_true;
}
```

`do while` El codigo se ejecuta minimo una vez

```csharp
do {
  instrucciones_while_condicion_es_true
} while (condicion)
```


### switch

```csharp
switch(expresion) {
  case x:
    // codigo
    break;
  case y:
    // codigo
    break;
  default:
    // codigo por defecto
    break;
}
```

---

## ARRAYS

* **Creacion de Arrays**

Una vez creado no se puede modificar el tamaño.  
Buenos para parametros o cosas fijas como nombres de los 12 meses  

**Array Literal**

```csharp
string[] coches;
string[] coches = {"Peugeot", "Opel", "Seat", "Mercedes"};
int[] numeros = {10, 20, 30, 40, 50};
```

**Array Constructor**

```csharp
// Crear array de 4 elementos y poner los valores luego
string[] coches= new string[4];

// Crear array de 4 elementos y añadir los valores a la vez
string[] coches= new string[4] {"Peugeot", "Opel", "Seat", "Mercedes"};

// Crear array de 4 elementos sin especificar el tamaño
string[] coches= new string[] {"Peugeot", "Opel", "Seat", "Mercedes"};

// Crear array de 4 elementos sin new ni especificar el tamaño
string[] coches= {"Peugeot", "Opel", "Seat", "Mercedes"};

// Si declaras un array y lo inicializas despues tendras que usar new
string[] coches;
coches= {"Peugeot", "Opel", "Seat", "Mercedes"}; // ERROR
coches= new string[] {"Peugeot", "Opel", "Seat", "Mercedes"}; 
```

* **Propiedades**

`array.Length` - nos ds el numero total de elementos del array  

```csharp
string[] misLetras = { "a", "z", "c" };
Console.WriteLine(misLetras.Length) // 3
```  

`array.Rank` - nos da el rango(numero de dimensiones) del array    

* **Metodos**

`array.Sort(nombreArray)` - ordena el array alfabeticamente o en orden ascendente de menor a mayor  

```csharp
string[] coches = {"Seat", "Audi", "Ford", "Renault"};
Array.Sort(coches);
foreach (string i in coches) {
  Console.WriteLine(i);
}
 
int[] misNumeros = {50, 30, 1, 8, 9}; 
Array.Sort(misNumeros);
foreach (int i in misNumeros) {
  Console.WriteLine(i);
}
```

`array.Reverse(nombreArray)` - invierte todos los elementos del array

```csharp
string[] misNumeros = { "a", "z", "c" };
Array.Reverse(misNumeros);
Console.WriteLine(misNumeros[0]);    // c
```
`array.Max-Min-Sum()` - calcula el valor minimo, maximo ,la suma de todos los elementos del array  

```csharp
using System.Linq;

int[] misNumeros = {50, 30, 1, 8, 9}; 
Array.Sort(misNumeros);
Console.WriteLine(misNumeros.Max());  // 50
Console.WriteLine(misNumeros.Min());  // 1
Console.WriteLine(misNumeros.Sum());  // 98
```

`array.IndexOf("elemento")` - Busca el elemento en el array y devuelve su posicion o -1 si no lo encuentra   

```csharp
string[] misNumeros = { "a", "z", "c" };
Console.WriteLine(Array.IndexOf(misNumeros, "z"));   // 1
```

* **Iterar**

---

## DICTIONARY

Pares `clave_unica-valor`  

* **Crear un Dictionaty**

```csharp
using System.Collections.Generic;

Dictionary<string, int> usuarios = new Dictionary<string, int>();  

Dictionary<string, int> usuarios = new Dictionary<string, int>() {
    { "Fulano", 57 },
    { "Mengano", 33 },
    { "Zutana", 18 },
    { "Manoli", 32 }
};  
```

* **Propiedades**

`diccionario.Count` - devuelve el numero total de elementos que existen    
`diccionario.keys` - devuelve una coleccion con todas las claves del dictionary  
`diccionario.Values` - devuelve una coleccion con todos los valores del dictionary    
`diccionario[clave]` - devuelve el valor asociado a la clave especificada  

* **Metodos**

`diccionario.Add(clave, valor)` - añade un par clave-valor al dictionary
`diccionario.Remove(clave)` - elimina el valor con clave especificada  
`diccionario.ContainsKey(clave)` - determina si la clave existe   
`diccionario.ContainsValue(valor)` - determina si el valor existe  
`diccionario.Clear()` - resetea el dictionary  
`diccionario.()` -  
`diccionario.()` -  

* **Iterar**

```csharp
foreach (string key in usuarios.Keys) {
  Console.WriteLine(key + " : " + usuarios[key]);
}
```

---

## LISTS

Los elementos estan en un orden especifico.

* **Crear una List**

```csharp
using System.Collections.Generic;

List<string> nombres = new List<string>();

List<string> nombres = new List<string>() {
    "Manolo",
    "Sonia",
    "Eva"
};
```

* **Propiedades**

`lista.Count` - numero de elementos que contiene 
`lista.Capacity` - maxima capacidad actual 
`lista[int32]` - devuelve el elemento en el indice int32     

* **Metodos**

`lista.Add(T item)` - Agrega un item al final de la Lista     
`lista.Insert(int index, T item)` - inserta T en el indice index de la Lista  
`lista.Clear()` - vacia toda la Lista     
`lista.Contains(T item)` - devuelve booleano si el elemento esta en la Lista  
`lista.Exists(Predicate<T> match)` - determina si la Lista contiene elementos que cumplen true con el predicado    
`lista.Find(Predicate<T> match)` - devuelve el primer elemento que cumple la condicion del predicado      
`lista.FindAll(Predicate<T> match)` - devuelve todos los elementos que cumplen la condicion del predicado    
`lista.FindIndex(Predicate<T> match)` - devuelve el indice del elemento que cumple la condicion o -1 si nadie la cumple    
`lista.FindLast(Predicate<T> match)` - devuelve el indice del ultimo elemento que cumple la condicion  
`lista.FindLastIndex(Predicate<T> match)` - devuelve el indice del ultimo elemento que cumple la condicion    
`lista.ForEach(el => Console.WriteLine(el))` - itera sobre la Lista   
`lista.IndexOf(T item)` - devuelve el indice del primer elemento que coincide con T   
`lista.Insert(int index, T item)` - inserta T en el indice index  
`lista.LastIndexOf(I item)` - devuelve el ultimo indice del elemento que coincide con T   
`lista.Remove(T item)` - elimina el elemento de valor T   
`lista.RemoveAt(int index)` - elimina el elemento en el indice int    
`lista.Reverse()` - Da la vuelta a todos los elementos  
`lista.Sort(Comparison<T>)` - Ordena los elementos segun el criterio o por defecto   
`lista.TrueForAll(Predicate<T> match)` - una condicion que todos cumplen y devuelve true (ejemplo predicado el => el%2 == 0)

* **Iterar**

```csharp
foreach (string el in usuarios) {
  Console.WriteLine(el);
}

usuarios.ForEach(el => Console.WriteLine(el));
```

---

## FILES

Para trabajar con archivos usamos la clase `File` del namespace `System.IO`  

```csharp
using System.IO;
```

`File.AppendText(String)` - añade el texto al archivo si existe o a uno nuevo si no existe   
`File.Copy(String, String)` - copia archivo existente a uno nuevo. No se puede sobreescribir  
`File.Copy(String, String, Boolean)` - copia archivo existente a uno nuevo. Se puede sobreescribir  
`File.Create(String)` - crea archivo nuevo sobreescribiendo incluso  
`File.Delete(String)` - Elimina el archivo   
`File.Exists(String)` - Determina si el archivo especificado existe   
`File.ReadAllLines(String)` - Abre el archivo, lee todas las lienas y lo cierra  
`File.ReadAllText(String)` - Abre el archivo, lo lee todo y lo cierra  
`File.Replace(string source, string dest, string Backup)` - Reemplaza el contenido del archivo especificado por el de otro archivo borrando el original y creando un backup del reemplazado  
`File.WriteAllText()` - Crea un nuevo archivo sobrreescribiendo si es preciso, escribe el contenido en el y cierra el archivo  

---




