# GOLANG STANDARD LIBRARY  

---

## FMT

`import "fmt"`

`fmt.Print()` - imprime  
`fmt.Println()` - imprime y salta de linea   
`fmt.Printf()` - imprime con un determinado formato  

```go
type point struct {
    x, y int
}
p := point{1, 2}

fmt.Printf("%v\n", p)                   // {1 2}

// en una struct, `%+v` incluye los nombres de los campos de la struct
fmt.Printf("%+v\n", p)                  // {x:1 y:2}

// Imprimir el tipo de un valor
fmt.Printf("%T\n", p)                   // main.point

// `%d` para enteros standard
fmt.Printf("%d\n", 123)                 // 123

// Imprime el caracter que corresponde al entero
fmt.Printf("%c\n", 33)                  // !

// Imprime floats
fmt.Printf("%f\n", 78.9)                // 78.90000

// Imprime string basicas `%s`.
fmt.Printf("%s\n", "\"string\"")        // "string"

// Imprime un puntero`%p`.
fmt.Printf("%p\n", &p)                  // 0x42135100
```

`fmt.Sprint()` - devuelve el resultado a una string    
`fmt.Sprintln()` - devuelve el resultado con salto de linea a una string    
`fmt.Sprintf()` - devuelve el resultado con un determinado formato a una string  

```go
// las Sxxx() son como las normales en vez de imprimir el resultado
// lo devuelven como un string
s := fmt.Sprintf("Hi, my name is %s and I'm %d years old.", "Bob", 23)
// s vale "Hi, my name is Bob and I'm 23 years old."
```

`fmt.Scan()` - para leer una palabra del teclado , almacena sucesivos valores
separados por un espacio en sucesivos argumentos. Saltos de linea cuentan como
espacio   
`fmt.Scanln()` - para leer una palabra del teclado , almacena sucesivos valores
separados por un espacio en sucesivos argumentos. Saltos de linea acaban con la
lectura de datos  

* **verbos**

\- General  
`%v` - valor en formato por defecto. En structs `%+v` añade los nombres de los campos  
`%T` - tipo del valor  
`%#v` - representacion del tipo del valor con sintaxis de golang  
\- Booleano   
`%t` - booleano, devuelve palabra true o false   
\- Integer   
`%b` - devuelve en base 2  
`%c` - devuelve caracter representado por el correspondiente codigo Unicode    
`%d` - devuelve en base 10   
`%U` - formato Unicode  
\- Floating point   
`f%` - notacion decimal sin exponentes    
`e%` - notacion decimal con exponentes
\- Strings y []byte
`%s` -  cadenas normales  
`%q` -  para escapar comillas dobles  
`%x` - convierte a base hexadecimal    

---

## STRINGS

`import "strings"`  

`strings.Contains("test", "es")` = true - Contiene "test" a "es"      
`strings.Count("test", "t")` = 2 - Cuantas "t" hay en "test"  
`strings.HasPrefix("test", "te")` = true - Comienza "test" por "te"    
`strings.HasSuffix("test", "st")` = True -  Acaba "test" en "st"    
`strings.Index("test", "e")` = 1 - Posicion de string "e" dentro de string
"test", si no esta devuelve -1  
`strings.Join([]string{"a","b"}, "-")` = "a-b" - Coge una lista de strings y
las junta en una separadas por otra string ("-" en el ejemplo)   
`strings.Repeat("a", 5)` = aaaaa -  Repite una string n veces  
`strings.Replace("aaaa", "a", "b", 2)` = "bbaa" - reemplaza en una cadena una
parte por otra n veces (o todas las que se pueda si pasamos -1)  
`strings.Split("a-b-c-d-e", "-")` = []string{"a","b","c","d","e"} - Parte una
string en una lista de strings usando otra string como separador  
`strings.ToLower("test")` = "TEST "- convierte la cadena a minusculas  
`strings.ToUpper("TEST")` = "test" - convierte la cadena a mayusculas  
`strings.Fields("cadena que sea)` = como split usando espacios en blanco . Equivalente a `strings.Split(text, " ")`  

Convertir string en slice of bytes y viceversa  
`arr := []byte("test")`  
`str := string([]byte{'t','e','s','t'})`

Fuera del paquete string

`len("aquiunacadena")` - nos da la longitud de la string     
`"cadena"[3]` - nos da el codigo ASCII del caracter de indice 3, "e" = 101  

---

## APPEND 

[Slice tricks](https://github.com/golang/go/wiki/SliceTricks)  


---

## IO

`import "io"`    

Tiene dos interfaces principales

* **Reader**  
soporta leer a a traves del metodo Read  

* **Writer**  
soporta escribir a traves del metodo Write

### io/ioutil

`import io/ioutil`    

* **leer un archivo**  

```go
func main() {
    bs, err := ioutil.ReadFile("test.txt")
    if err != nil {
        return
    }
    str := string(bs)
    fmt.Println(str)
}
```

---

## OS

`import "os"`  

### Archivos

* **leer un archivo**  

```go
func main() {
    file, err := os.Open("test.txt")
    if err != nil {
        // handle the error here
        return
    }
    defer file.Close()
    stat, err := file.Stat()              // get the file size
    if err != nil {
        return
    }
    bs := make([]byte, stat.Size())       // read the file
    _, err = file.Read(bs)
    if err != nil {
        return
    }
    str := string(bs)
    fmt.Println(str)
}
```

* **crear un archivo**  

```go
func main() {
  file, err := os.Create("test.txt")
  if err != nil {
      // handle the error here
      return
  }
  defer file.Close()
  file.WriteString("test")
}
```

* **Leer el contenido de un directorio**  

`Readdir` - coge un argumento que es el numero de entradas que devuelve. Con -1
devuelve todas  

```go
func main() {
    dir, err := os.Open(".")
    if err != nil {
        return
    }
    defer dir.Close()
    fileInfos, err := dir.Readdir(-1)
    if err != nil {
        return
    }
    for _, fi := range fileInfos {
        fmt.Println(fi.Name())
    }
}
```

`Walk` - para recorrer recursivamente un directorio. Pertenece al paquete
[path/filepath](#pathfilepath)  

### Argumentos en comandos

* **Command line arguments**

el primer valor del slice de argumentos es el path del programa

`argsWithProg := os.Args`- slice completo path incluido  
`argsWithoutProg := os.Args[1:]` - slice solo de argumentos  
`arg := os.Args[x]` - devuelve argumento de posicion X  

### Variables de Entorno

* **environment variables**  

`os.Setenv("nombreVariable", "valor")` - establece un par clave/valor para una variable de entorno  
`os.Getenv("nombreVariable")` - devuelve el valor de esa clave  

```go
// os.Environ es una lista de todas las variables de entorno
for _, e := range os.Environ() {
    pair := strings.Split(e, "=")
		fmt.Println(pair[0], "-->", pair[1])
}
```

---

## PATH/FILEPATH  

`import path/filepath`    

* **Recorrer recursivamente un directorio**

`Walk`  

```go
func main() {
    filepath.Walk(".", func(path string, info os.FileInfo, err error)
                                                              error {
        fmt.Println(path)
        return nil
    })
}
```
## REGEXP  

`import "regexp"`  

```go
// Comprueba si es una cadena
patron := "loquequeremoscomprobar"
match, _ := regexp.MatchString("p([a-z]+)ch", patron)
fmt.Println(match)

// o compilamos primero una struct optimizada para regexp
patron := "loquequeremoscomprobar"
r, _ := regexp.Compile("p([a-z]+)ch")
fmt.Println(r.MatchString(patron))
```

---

## JSON

`import "encoding/json"`  

[Golang JSON](https://blog.golang.org/json-and-go)  

`json.Marshal(struct object)` - Go data to JSON  
`json.Unmarshal(dataJson, &structObject)` - JSON to Go data  

---

## TIME

`import "time"`  

`now := time.Now()` - nos da la hora actual 2012-10-31 15:50:13.793654 +0000 UTC

`then := time.Date(2015, 10, 10, 18, 30, 08, 0, time.UTC)` - Creamos un struct de tiempo asociado a una localizacion (time zone)   

```go
then.Year()
then.Month()
then.Day()
then.Hour()
then.Minute()
then.Second()
then.Nanosecond()
then.Location()
then.Weekday()
then.Before(now)
then.After(now)
then.Equal(now)
```

`diff := now.Sub(then)` - metodo Sub devuelve duracion del intervalo entre dos tiempos  

```go
diff.Hours()
diff.Minutes()
diff.Seconds()
diff.Nanoseconds()
// avanzar o retroceder en el tiempo
then.Add(diff)
then.Add(-diff)
```

### Timestamp

`Unix epoch`

```go
now := time.Now()
secs := now.Unix()
nanos := now.UnixNano()
millis := nanos / 1000000

time.Unix(secs, 0)
time.Unix(0, nanos)
```

### Intervalos

`timers` - para hacer algo una vez dentro de un tiempo  

```go
timer1 := time.NewTimer(time.Second * 2)
<-timer1.C
fmt.Println("Timer 1 expired")
timer2 := time.NewTimer(time.Second)
go func() {
    <-timer2.C
    fmt.Println("Timer 2 expired")
}()
stop2 := timer2.Stop()
if stop2 {
    fmt.Println("Timer 2 stopped")
}
// Timer 1 expired
// Timer 2 stopped
// Timer 2 expired NUNCA APARECE, se cancela antes
```

`tickers` -  para hacer algo repetidamente a intervalos regulares  

```go
ticker := time.NewTicker(time.Millisecond * 500)
go func() {
    for t := range ticker.C {
        fmt.Println("Tick at", t)
    }
}()
time.Sleep(time.Millisecond * 1600)
ticker.Stop()
fmt.Println("Ticker stopped")
```

---

## MATH/RAND

`import "math/rand"`  

`rand.Intn(10)`- genera un numero aleatorio entero >= 0 y < 10  
`rand.Float64()`- genera un numero aleatorio >= 0.0 y < 1.0  
`(rand.Float64()*5)+5`- genera entre >= 5.0 y < 10.0   

`s1 := rand.NewSource(time.Now().UnixNano())`- semilla para que no sea siempre igual  
`r1 := rand.New(s1)`- para ir cambiando la semilla  

---

## STRCONV

`import "strconv"` - conversiones entre numeros y strings  

`i, err := strconv.Atoi("-42")` - string to int  
`s, err := strconv.Itoa(-42)` - int to string  
`b, err := strconv.ParseBool("true")` - string to boolean  
`f, err := strconv.ParseFloat("3.1415", 64)` - string to float  
`i, err := strconv.ParseInt("-42", 10, 64)` - string to int  
`u, err := strconv.ParseUint("42", 10, 64)` - string to uint  
`s := strconv.FormatBool(true)` - boolean value to string  
`s := strconv.FormatFloat(3.1415, 'E', -1, 64)` - float to string  
`s := strconv.FormatInt(-42, 16)` - int to string  
`s := strconv.FormatUint(42, 16)` - uint to string  

---

## FLAG

`import "flag"`  

Para enviar argumentos a un comando  

[Ejemplos](https://gobyexample.com/command-line-flags)  

---

## CONTAINERS/LIST  

`import containers/list`    

Este paquete contiene una doubly linked list  

>> Linked List --> ![go](/z-static/images/go/linkedList.png)

Cada node la de lista contiene un valor y un puntero al siguiente nodo. Como
esta lista es doblemente enlazada cada nodo tiene tambien un puntero al nodo
anterior

---

## SORT  

`import sort`    


Contiene funciones para ordenar datos arbitrario de slices de ints y floats y
structs definidas por el usuario  

* **s = []strings**

`sort.strings(s)` -  De menor a mayor alfabeticamente  

* ** n = []ints || float32 || float64**

`sort.Ints(n)` -  Ordena los numeros de menor a mayor  
`sort.IntsAreSorted(n)` -  booleano que devuelve si estan ordenados  

* **custom sorting**

```go
package main

import "sort"
import "fmt"

// If I have an array/slice of structs in Go and want to sort them
// using the sort package it seems to me that I need to implement
// the whole sort interface which contains 3 methods:
// https://golang.org/pkg/sort/#Interface  

type ByLength []string

func (s ByLength) Len() int {
    return len(s)
}

func (s ByLength) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

func (s ByLength) Less(i, j int) bool {
    return len(s[i]) < len(s[j])
}

func main() {
    fruits := []string{"peach", "banana", "kiwi"}
    sort.Sort(ByLength(fruits))
    fmt.Println(fruits)
}
```

---

## HASH/CRC32

`import hash/crc32`    


1B-85

---

## CRYPTO/SHA1

`import crypto/sha1`    

[SHA1 ejemplo](https://gobyexample.com/sha1-hashes)


1B-86

---