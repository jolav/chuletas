# GOLANG 1.7.4

---

## INSTALACION

Ademas de lo del manual añadir en `nano /home/user/.bashrc` y `nano /home/user/.profile`

```sh
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/code/go
// OPCIONAL para tener disponibles los binarios compilados
export PATH=$PATH:$GOPATH/bin
```

recargar con `source ~/.profile`   

Carpetas que se crean:  

* `bin` - Contiene los binarios compilados. Podemos añadir la carpeta `bin` al path del sistema para hacer los binarios compilados ejecutables desde cualquier  sitio  
* `pkg` - contiene los versiones compiladas de las librerias disponibles para
que el compilador las pueda enlazar sin tener que recompilarlas  
* `src` -  contiene todo el codigo organizado por rutas de import  

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
> `!=` no igual  
> `>` mayor que  
> `<` menor que  
> `>=` mayor o igual que  
> `<=` menor o igual que  

* **Logicos**

> `&&` AND  
> `||` OR  
> `!` NOT  

* **Punteros**

> `&` devuelve la direccion de una variable  
> `*` puntero a una variable  

---

## VARIABLES

Una variable puede contener cualquier tipo, incluso una funcion

```go
func main() {
    accion := func() {
        fmt.Println("Hola")
    }
    accion()
}
```

### Zero Values

Cuando se declaran variables sin un valor explicito se les asigna el valor zero  

>>> `int` - 0  
>>> `float` - 0.0   
>>> `string` - ""   
>>> `boolean` - false  
>>> `pointers` - nil  
>>> `map` - nil  
>>> `slices` - nil  
>>> `array` - array listo para usar con sus elementos a zero value que sea  
>>> `functions` - nil
>>> `interfaces` - nil  
>>> `channels` -nil  

### Declaracion

* **Declaracion de variables**

```go
var (
    name      string
    age       int
    location  string
)
var (
    name, location  string
    age             int
)
var name string
```

* **Inicializacion de variables**

```go
var (
    name      string  = "brusbilis"
    age       int     = 100
)
var (  // inferred typing
    name = "brusbilis"
    age  = 32
)
var name, location, age = "brusbilis", "casa", 100
```

* **Sentencia de asignacion `:=`**  
Dentro de una funcion podemos usar `:=` en lugar de `var`  

```go
func main() {
    name, location := "brusbilis", "casa"
    age := 100
}
```

* **new**

Pone a cero el valor del tipo y devuelve un puntero a el.

```go
x := new(int)
```

* **make**

Necesario para `slices` `maps` y `channels`  

### Alcance

El alcance es la region del programa donde una variable definida existe  
Tipos de variables segun donde se declaren:  

* `local variables` - dentro de una funcion o un bloque. Fuera de ese entorno
no existen    
* `global variables` - fuera de todas las funciones o bloques. Accesibles desde
cualquier parte del programa  
* `formal parameters` - en la definicion de los parametros de una funcion. Se
tratan como locales para esa funcion y tienen preferencia sobre las globales    

Cuando coinciden dentro de una funcion o bloque una local y una global prevalece
la local  

### Conversion de tipos

Go no tiene conversion implicita de tipos  
`T(v)` - Convierte el valor `v` al tipo `T`  

```go
i := 42
f := float64(i)
u := uint(f)
```

### Type Assertion

---

## DATOS BASICOS

![go](/z-static/images/go/basicTypes.png)

### Numeros

* **Integers**

Los enteros son numeros sin decimal

`int` - positivos y negativos    
`uint` - unsigned, solo los positivos  
`byte` - alias de uint8 (0-255)  
`rune` - alias de int32   

* **Numeros de Punto Flotante**

Son numeros reales (con parte decimal)  

`float32` - conocido como simple precision  
`float64` - conocido como doble precision  

* **Numeros Complejos**

`complex64` - parte real float32 + partes imaginarias  
`complex128` - parte real float64 + partes imaginarias  

### Booleanos

`&&` - and  
`||` - or   
`!` - not  

> ![go](/z-static/images/go/boolean1.png)
> ![go](/z-static/images/go/boolean2.png)
> ![go](/z-static/images/go/boolean3.png)


### Strings

Estan hechas de bytes (uno por caracter)  
La diferencia entre comillas simples o dobles es que en estas no pueden contener
nuevas lineas y se permiten escapar caracteres especiales  

`len(string)` - longitud de la cadena  
`"Hola mundo"[1]` - acceder a caracteres de la cadena  
`"Hello, " + World"`  

### Constantes

Se declaran como variables pero con la palabra clave `const`.  
No se pueden declarar usando `:=`  
Solo pueden ser caracteres, string, booleano o valores numericos.   

```go
const PI = 3.14
```

### Iota

[iota info](https://splice.com/blog/iota-elegant-constants-golang/)

Es un identificador usado en declaraciones de constantes para indicar que son
autoincrementables.  .
Se resetea a cero cuando aparece la palabra reservada `const`

```go
const ( // iota is reset to 0
    c0 = iota  // c0 == 0
    c1 = iota  // c1 == 1
    c2 = iota  // c2 == 2
)
```

---

## POINTERS y MUTABILIDAD

### Punteros

Por defecto Go pasa los argumentos por valor (crea una copia)  
Para pasarlos por referencia hay que pasar punteros o usar estructuras de
datos que usan valores por referencia como slices y maps.  

`&` - para conseguir el puntero de un valor lo ponemos delante de su nombre  
`*` - para desreferenciar un puntero y que nos de acceso a su valor  

```go
i := 42
p := &i             // Genera un puntero a i
fmt.Println(*p)     // lee i a traves del puntero p
*p = 21             // establece i a traves del puntero p
```

```go
func zero(x *int) {
    *x = 0
}
func main() {
    x := 5
    zero(&x)  
    fmt.Println(x) // x is 0
}
```

* **new**

`new` - coge un tipo como argumento, asigna suficiente memoria para ese tipo
de dato y devuelve un puntero que apunta a esa memoria. Luego el GC (garbage
collector lo limpia todo)  

```go
func zero(x *int) {
    *x = 5
}
func main() {
    x := new(int)
    zero(x)  
    fmt.Println(*x) // x is 5
}
```

### Mutabilidad

Solo las constantes son inmutables.  
Sin embargo como los argumentos se pasan por valor, una funcion que recibe y
modifica un argumento no muta el valor original  

* Ejemplo

```go
func addOne(x int) {
	x++
}
func main() {
	x := 0
	addOne(x)
	fmt.Println(x)         // x da 0
}
```

```go
// Si usamos punteros
func addOne(x *int) {
	*x++
}
func main() {
	x := 0
	addOne(&x)
	fmt.Println(x)          // x da 1
}
```

### POINTERS vs VALUE  

[Leer esto](https://stackoverflow.com/questions/23542989/pointers-vs-values-in-parameters-and-return-values)   

---
## ESTRUCTURAS DE CONTROL

### for

```go
// for normal
sum := 0
for i := 0; i < 10; i++ {
  sum = sum + i
}

// for sin declaraciones pre/post
sum := 1
for ; sum < 1000; {
  sum = sum + sum
}

// for como un while
sum := 1
for sum < 1000 {
  sum = sum + sum
}

// for infinito
for {
    ..codigo
}
```

### if

```go
if answer != 42 {
    return "Wrong answer"
}
if err := foo(); err != nil {
    panic(err)
}
if {
else {
}
```

### switch

> `switch` en golang  

* Solo se pueden comparar valores del mismo tipo  
* declaracion `default` para ejecutarse si todas las demas fallan  
* en la declaracion se puede usar una expression (pej calcular un valor)  
`case 3 - 2:`  
* Se puede tener multiples valores un solo caso  
`case 6, 7:`  
* `fallthroguh` se ejecutan todas las declaraciones que cumplen la condicion    
* `break` sale del switch  

```go
func main() {
    n := 1
    switch n {
    case 0:
        fmt.Println("is zero")
        fallthrough
    case 1:
        fmt.Println("<= 1")
        fallthrough
    case 2:
        fmt.Println("<= 2")
        fallthrough
    case 3:
        fmt.Println("<= 3")
        if time.Now().Unix()%2 == 0 {
            fmt.Println("un pasito pa lante maria")
            break
        }
       fallthrough
    case 4:
        fmt.Println("<= 4")
    default:
        fmt.Println("Try again!")
    }

}
```

### range

Itera sobre los `slice` o `map`  

* **slice**

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
func main() {
    for i, v := range pow {
    		fmt.Println("Posicion", i, "valor", v)
    }
}
```

Podemos omitir el index usando `_`  
Podemos omitir el valor omitiendo por completo `, value`  

```go
func main() {
    pow := make([]int, 10)
    for i := range pow {
        pow[i] = 1 << uint(i)
    }
    for _, value := range pow {
        fmt.Printf("%d\n", value)
    }
}
```

* **map**

El primer parametro no es un entero autoincrementable sino la clave del map  

`for key, value := range cities`

* **break**

Paras la iteracion en cualquier momento  

* **continue**

Omites una iteracion  

---

## ARRAYS

**`tipo [n]T` - es un array de `n` elementos de tipo `T`**    

* No se pueden redimensionar  
* Se pueden inicializar al declararlos  
    `a := [2]string{"hello", "world!"}`  
    `a := [...]string{"hello", "world!"}` ´o incluso usando una ellipsis  
* `printìng arrays`  
    `fmt.Printf("%q\n", a)    // ["hello" "world!"]`
* `len(array)`  


---

## SLICES

**`tipo []T` - es un slice de elementos de tipo `T`**  

* **Crear un slice :**  

Los slice hay que crearlos antes de usarlos

> * `slice literal`    
`mySlice := []int{2, 3, 5, 7, 11, 13}`
> * `make` - crea un slice vacio de una longitud y (opcional una capacidad)    
`cities := make([]string, len, cap)`  

* **Recortando un slice**

`s[a:b]` - selecciona elementos desde la pos a (inclusive) hasta b (exclusive)  
`s[:b]` - un indice `a` que no se declara es un `0` implicito  
`s[a:]` - un indice `b` que no se declara es implicito un `len(s)`   

* **Añadiendo a un slice**

`cities = append(cities, "San Diego")`  
`cities = append(cities, "San Diego", "Mountain View")`  
`otherCities := []string{"Santa Monica", "Venice"}`  
`cities = append(cities, otherCities...)`  

* **Copiar un slice**

`copy(destino, origen)`  

* **Length**

`len(slice)`  

* **Nil slices**

`var z []int` - El valor cero de un slice es nil. Un slice nil tiene una
longitud de cero

---

## MAPS

**`tipo map[a]b - es un map de claves tipo a con valores tipo b`**   

Formado por `pares clave/valor`  

* **Crear un map :**  

Los map hay que crearlos antes de usarlos

> * `map literal`    
`amigos := map[string]int{"Juan":50, "Elena":21, "Carlos":41,}`
> * `make` - creas un nil map vacio  
`amigos := make(map[string]int)`  

* **Modificando maps**

`m[key] = elem` - Insertando o actualizando un valor  
`elem = m[key]` - Devuelve el elemento  
`delete(m, key)` - Borrando un elemento   
`elem, ok = m[key]` - Testea si existe un valor con una clave determinada  

```go
elements := map[string]map[string]string{
    "H": map[string]string{
        "name":"Hydrogen",
        "state":"gas",
    },
    "He": map[string]string{
        "name":"Helium",
        "state":"gas",
    },
    "Li": map[string]string{
        "name":"Lithium",
        "state":"solid",
    },
}
if el, ok := elements["Li"]; ok {
    fmt.Println(el["name"], el["state"])
}
```

---

## STRUCTS

Es una coleccion de campos/propiedades  
Solo los campos exportados (primera letra mayuscula) son accesibles de fuera del paquete  

### Inicializacion

```go
type Circle struct {
    x, y, r float64
}
```

`var c Circle` - crea una variable local Circle que pone por defecto los
valores a cero (0 para int, 0.0 para float. "" para string, nil para punteros)  
`c := new(Circle)` - asigna memoria para todos los campos, los inicializa a
cero y devuelve un puntero a la struct (*Circle), los punteros se usan mucho en
structs paa que las funciones puedan modificar los datos.  
`c := Circle{x: 0, y: 0, r: 5}`  
`c := Circle{0, 0, 5}`  
`c := &Circle{0, 0, 5}`  
`c := Circle{x: 1}`  
`c := Circle{}`  

```go
type Circle struct {
    x, y, r float64
}
func main() {
    fmt.Println(c.x, c.y, c.r)
    c.x = 10
    c.y = 5
}
```

### Metodos

El receptor del metodo esta entre la palabra clave `function` y el nombre del
metodo

`func (u User) Greeting() string` - nos permite llamarla con u.Greeting()  

* **Organizacion del codigo**

```go
package models

// list of packages to import

// list of constants

// list of variables

// Main type(s) for the file,
// try to keep the lowest amount of structs per file when possible.

// List of functions

// List of methods
```

* **Alias**

Para definir metodos en un tipo que no es tuyo se usan alias

```go
import "strings"
type MyStr string
func (s MyStr) Uppercase() string {
    return strings.ToUpper(string(s))
}
func main() {
    fmt.Println(MyStr("test").Uppercase())
}
```

* **Usar punteros en los receptores**

Los metodos se pueden asociar a un nombre o a puntero. Ventajas de usar
punteros:  

* evitar copiar el valor con cada llamada al metodo (pasarlo por referencia)  
* para poder modificar el valor que pasamos  

### Composicion

```go
type User struct {
    Id              int
    Name, Location  string
}
type Player struct {
    User
    GameId  int
}
```

Podemos acceder a la Struct de User:  
`a := new(Player)`  
`a.User.Name`  
`a.Name`


---

## INTERFACES

* Es un conjunto de metodos  
* Puede tener cualquier valor que implementen esos metodos  

```go
type usuario struct {
	nombre, apellido string
}

func (u usuario) getNombre() string {
	return fmt.Sprintf("%s %s", u.nombre, u.apellido)
}

type cliente struct {
	Id             int
	nombreCompleto string
}

func (c *cliente) getNombre() string {
	return c.nombreCompleto
}

type llamar interface {
	getNombre() string
}

func saludar(n llamar) string {
	return fmt.Sprintf("Sr %s", n.getNombre())
}

func main() {
	u := usuario{"nombre", "apellido"}
	fmt.Println(saludar(u))
	c := &cliente{42, "nombre2"}
	fmt.Println(saludar(c))
}
```

---

## FUNCTIONS

### Call Stack

```go
func main() {
    fmt.Println(f1())
}
func f1() int {
    return f2()
}
func f2() int {
    return 1
}
```

>>> ![go](/z-static/images/go/callStack.png)

### Argumentos

* Argumentos que reciben. Las funciones pueden recibir 0 o mas argumentos todos tipados despues del nombre de la variable.    

```go
func add(x int, y int) int {
    return x + y
}
func add(x, y int) int {   // int afecta a todos los parametros (x, y)
    return x + y
}
```

```go
func add(args ...int) int {
    total := 0
    for _, v := range args {
        total += v
    }
    return total
}
func main() {         // pasamos los parametros que queramos
    fmt.Println(add(1,2,3))
    xs := []int{1,2,3}
    fmt.Println(add(xs...)) // tambien podemos pasar un slice
}
```

* Retorno de parametros, puede devolver cualquier numero de ellos  

```go
return region, continente   // devuelve mas de un valor
// Si los parametros de retorno estan nombrados vale con solo return
func location(name, city string) (region, continent string) {
    ..codigo
    return    // devuelve region y continent
}
```

### Closures

```go
func generadorPares() func() uint {
    i := uint(0)
    return func() (ret uint) {
        ret = i
        i = i + 2
        return
    }
}
func main() {
    nextPar := generadorPares()
    fmt.Println(nextPar()) // 0
    fmt.Println(nextPar()) // 2
    fmt.Println(nextPar()) // 4
}
```

### Recursion

```go
func factorial(x uint) uint {   
    if x == 0 {
        return 1
    }
    return x * factorial(x-1)
}
```

### defer

Aplaza la ejecucion de una funcion hasta que termina la funcion en la que esta  

```go
func main() {
    defer fmt.Println("world")
    fmt.Println("hello")
}
```

Se usa para liberar recursos cuando se pueda

```go
f, _ := os.Open(filename)
defer f.Close()
```

### panic, recover

`panic("valor de panic")` - crea un runtime error .   
`recover()` - detiene el panic y devuelve el valor que fue pasado con la
llamada a panic

Un `panic` generalmente indica un error de programacion o una condicion
excepcional de la que no hay forma facil de recuperarse  

```go
func main() {
    defer func() {
        str := recover()
        fmt.Println(str)
    }()
    panic("PANIC")
}
```

---

## CONCURRENCY

### goroutines

`go f(x)` comienza la ejecucion de una nueva goroutine que es una funcion capaz
de ejecutarse concurrentemente con otras funciones.  

```go
func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}

func main() {
    go say("world")
    say("hello")
}
```

### channels

`channels` - son un conducto a traves del cual puedes recibir y enviar datos con
el operador `<-`  

`ch <- data` - Envia data al canal ch  
`data := <-ch` - Recibe informacion del canal ch y lo asigna a data  

`ch := make(chan int)` - Los canales hay que crearlos antes de usarlos  

Por defecto los envios y recepciones esperan hasta que el otro lado este listo.
Esto permite a las goroutines sincronizarse sin bloqueos especificos o
condiciones  

```go
func sum(a []int, c chan int) {
    sum := 0
    for _, v := range a {
        sum += v
    }
    c <- sum // send sum to c
}

func main() {
    a := []int{7, 2, 8, -9, 4, 0}
    c := make(chan int)
    go sum(a[:len(a)/2], c)
    go sum(a[len(a)/2:], c)
    x, y := <-c, <-c // receive from c
    fmt.Println(x, y, x+y)
}
```

* **Buffered channels**

`ch := make(chan int, 100)` - Ponemos un buffer a los canales indicando su
longitud como segundo argumento en el `make` para inicializar el canal  
Enviar datos aun canal con buffer se bloquea si el buffer esta lleno   
Recibir datos de un canal con  buffer se bloquea si el buffer esta vacio  

```go
func main() {
	c := make(chan int, 2)
	c <- 1;    c <- 2;   c <- 3
	fmt.Println(<-c);    fmt.Println(<-c);   fmt.Println(<-c)
}       // fatal error: all goroutines are asleep - deadlock!
```

Sin embargo el siguiente funcionaria. Al añadir el valor extra desde una
goroutine no se bloquea el hilo principal pues aunque la goroutine se llama
antes que el canal se vacie esta esperara hasta que haya espacio en el canal.  

```go
func main() {
  	c := make(chan int, 2)
  	c <- 1;		c <- 2
  	c3 := func() { c <- 3 }
  	go c3()
  	fmt.Println(<-c);    fmt.Println(<-c);    fmt.Println(<-c)
}
```

* **Close**

`close(ch)` - Solo un emisor puede cerrar un canal. Enviar a un canal cerrado
causa un panic. No son como ficheros que hace falta cerrarlos. Solo se cierran
para avisar al receptor de que no llegaran mas datos y para terminar los loop
range      

`v, ok := <-ch` - Un emisor puede cerrar un canal para indicar que no se enviara nada mas. Los receptores pueden testear cuando un canal ha sido cerrado
asignando un segundo parametro a la expresion receptora  
`ok` sera falso cuando no haya mas valores que recibir y el canal este cerrado.  

`for i := range ch` - recibe valores del canal hasta que se cierre

* **Select**

Es como `switch` pero con canales  

1A - 89  
1B - 103  

---

## PACKAGES

Un programa Go esta hecho con paquetes. Los programas empiezan ejecutando la
funcion `main` dentro del paquete `main`.  
Por convencion el nombre del paquete es la ultima palabra de la ruta del import.  
El paquete "math/rand" comprende archivos que comienzan con la sentencia
`package rand`  

Paquetes que no son de la libreria estandar se importan usando una URL web,
pero antes hay que descargarlos con `go get`

```go
go get github.com/creador/ruta/al/paquete
import "github.com/creador/ruta/al/paquete"
```

Despues de importar un paquete podemos usar los nombres que el exporta (sean
variables, metodos o funciones) desde fuera de el paquete.
Los nombres exportados en Go comienzan por letra mayuscula

```go
package main
import ( "fmt"	"math" )
func main() {
	fmt.Println(math.Pi)
	fmt.Println(math.pi)
}
// cannot refer to unexported name math.pi
```

>> **Otras formas de importar paquetes**

* `ìmport alias "fmt"` - Crea un alias de fmt. Ahora es alias.LoQueSea en lugar de fmt.LoQueSea  
* `import . "fmt"` -  Permite acceder al contenido directamente sin tener que
ir precedido de fmt  
* `import _ "fmt"` - Elimina las advertencia del compilado sobre ese paquete
si no se usa y  ejecuta si hay las funciones de inicializacion, El resto del
paquete permanece inaccesible.  

### Crear paquetes  

Los nombres de paquetes coinciden con la carpeta donde estan. Esto se puede
cambiar pero no merece la pena  
Por convencion el nombre del paquete es la ultima palabra de la ruta del import.  

`~/src/proyectoX/main.go`

```go
package main
import "fmt"
import "proyectoX/utilidades"  // la ruta es a partir de srcs
func main() {
    // llamada a  utilidades.Media(xs)
}
```

`~/src/proyectoX/utilidades/media.go`

```go
package utilidades
func Media() {
    // codigo que sea
}
```

### Desinstalar paquetes

`go clean -i ruta/paquete...` - teoricamente borras los pkg y bin, los src hay que borrarlos manualmente  

### Actualizar

`go get -u all` -  Actualiza todos  

`go get -u full/package/name` -  Actualizar solo ese paquete

---

## EJECUCION

El programa se inicia por la funcion `main` del `package main`  

Antes se ejecutan las funciones `init` de ese fichero

Los paquetes importados "_ import "ruta/paquete" hacen que el compilador acepte un paquete que no se usa y ademas ejecutan la o las funciones `init` de ese paquete  

---

## TESTING

El compilador ignora todos los archivos que terminan en `_test.go`  

`~/src/proyectoX/utilidades/media_test.go`

```go
package utilidades

import "testing"

type testpair struct {
  	values  []float64
  	average float64
}

var tests = []testpair{
  	{[]float64{1, 2}, 1.5},
  	{[]float64{1, 1, 1, 1, 1, 1}, 1},
  	{[]float64{-1, 1}, 0},
}

func TestAverage(t *testing.T) {
    for _, pair := range tests {
        v := Media(pair.values)
        if v != pair.average {
            t.Error(
                "For", pair.values,
                "expected", pair.average,
                "got", v,
            )
        }
    }
}
```

`go test`

---

## REFLECTION

---

## DEPLOY  

* **Cross Compile**

`GOOS=linux GOARCH=amd64 go build`  

* **ejecutar**

`./binarioGo &`- la & hace que se libere la consola  


### Systemd

`systemctl status servidorGO` nos da informacion del servicio

`Service name`: servidorGO.service

```sh
/etc/systemd/servidorGO.service
/etc/systemd/system/servidorGO.service
/etc/systemd/system/multi-user.target.wants/servidorGO.service
/sys/fs/cgroup/systemd/system.slice/servidorGO.service
```

Systemd configuration  
`nano /etc/systemd/servidorGO.service`

```sh
[Unit]
Description=Webhook

[Service]
User=brus
Group=www-data
Restart=on-failure                    // o always ???
ExecStart=/home/brus/Go/src/Pruebas/servidor //servidor es el binario

[Install]
WantedBy=multi-user.target
```

* Activar el servicio y arrancar automaticamente al inicio    
`systemctl enable application.service`
* Desactivar el servicio de iniciar automaticamente   
`systemctl disable application.service`  

[Buena guia de systemctl](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)

---

## ERRORS

[Errores en Go](https://blog.golang.org/error-handling-and-go)

Los captura un tipo interfaz predefinido cuyo unico metodo `Error` devuelve
una cadena  

```go
type error interface {
    Error() string
}
```

Forma estandard de tratar los errores.  
`log.Fatal(err)` - manda el error a la terminal y detiene el programa  

```go
f, err := os.Open("filename.ext")
if err != nil {
    log.Fatal(err)
}
```

Podemos aligerar la repeticion un poco usando:  

```go
func check(e error) {
    if e != nil {
        panic(e)
    }
}
// y ahora ya solo ponemos
check(err)
```

---



