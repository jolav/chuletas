# GOLANG 1.6 STANDARD LIBRARY

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

Convertir string en slice of bytes y viceversa  
`arr := []byte("test")`  
`str := string([]byte{'t','e','s','t'})`   

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

---

## NET/HTTP

`import net/http`    

---

## HTML/TEMPLATE

`import html/template`    

* Para usarlo hay que importar el paquete `html/template`
* crear la plantilla `t, _ := template.ParseFiles("index.html")`
* asignar valor a variables de plantilla `template_value := "Hola"`
* servir la pagina `t.Execute(w, template_values)`

index.html

```html
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>Hello World</title>
</head>
<body>
Hello {{ . }}
</body>
</html>
```

pagina.go

```go
package main

import (
	"html/template"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	t, _ := template.ParseFiles("index.html")
	name := "World"
	t.Execute(w, name)
}

func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}
```

### Fields

* `{{}}` cualquier cosa a ser renderizada debe ir entre dobles parentesis

* `{{.}}` abreviatura para el objeto actual

* `{{ FieldName}}` campo FieldName del objecto actual

* Arrays and slices

```go
type Friend struct {
    Fname string
}

type Person struct {
    UserName string
    Emails   []string
    Friends  []*Friend
}

func main() {
    f1 := Friend{Fname: "minux.ma"}
    f2 := Friend{Fname: "xushiwei"}
    t := template.New("fieldname example")
    t, _ = t.Parse(`hello {{.UserName}}!
            {{range .Emails}}
                an email {{.}}
            {{end}}
            {{with .Friends}}
            {{range .}}
                my friend name is {{.Fname}}
            {{end}}
            {{end}}
            `)
    p := Person{UserName: "Astaxie",
        Emails:  []string{"astaxie@beego.me", "astaxie@gmail.com"},
        Friends: []*Friend{&f1, &f2}}
    t.Execute(os.Stdout, p)
}
```

* Arrays and slices index

```go
const tmpl = `
{{range $index, $link := .}}
{{$index}}: <a href="{{$link.Href}}">{{$link.Name}}</a>
{{end}}
`

type Link struct {
    Name string
    Href string
}

func main() {
  	// arrays
  	var la [2]Link
  	la[0] = Link{"Google", "https://www.google.com/"}
  	la[1] = Link{"Facebook", "https://www.facebook.com/"}
  	t, _ := template.New("foo").Parse(tmpl)
  	t.Execute(os.Stdout, la)

  	// slices
  	var ls []Link
  	ls = append(ls, Link{"Google", "https://www.google.com/"})
  	ls = append(ls, Link{"Facebook", "https://www.facebook.com/"})
  	t.Execute(os.Stdout, ls)
}
```

* Maps

```go
const tmpl = `
{{range $name, $href := .}}
<a href="{{$href}}">{{$name}}</a>
{{end}}
`

func main() {
  	// map
  	var m = map[string]string{
  	    "Google": "https://www.google.com/",
  	    "Facebook": "https://www.facebook.com/",
  	}
  	t, _ := template.New("foo").Parse(tmpl)
  	t.Execute(os.Stdout, m)
}
```

### Conditions

* {{if}} {{else}} : Solo para valores booleanos, no hace comparaciones

```go
{{if ``}}
    Will not print.
{{end}}
```

```go
{{if `anything`}}
    Will print.
{{end}}
```

```go
{{if `anything`}}
    Print IF part.  
{{else}}
  Print ELSE part.
{{end}}
```

### Pipelines

* {{ . | html}} Por ejemplo usamos esto para coger el objto actual '.' y
aplicarle escape a HTML al objeto


### Variables

```go
texto := "{{with $3 := `hello`}}{{$3}}{{end}}!\n"
t := template.Must(template.New("name").Parse(texto))
t.Execute(os.Stdout, nil)                  
```

// Resultado -> hello!

```go
texto1 := "{{with $x3 := `hola`}}{{$x3}}{{end}}!\n"
t1 := template.Must(template.New("name1").Parse(texto1))
t1.Execute(os.Stdout, nil)
```

// Resultado -> hola!

```go
texto2 := "{{with $x_1 := `hey`}}{{$x_1}} {{.}} {{$x_1}}{{end}}!\n"
t2 := template.Must(template.New("name2").Parse(texto2))
t2.Execute(os.Stdout, nil)
```

// Resultado -> hey hey hey!

### Funciones

#### Predefinidas

Por ejemplo `print` equivale a `fmt.Sprintf`

```go
func main() {
    texto := "{{with $x := `hello`}}{{printf `%s %s` $x `Mary`}}
        {{end}}!\n"
    t := template.New("test")
    t = template.Must(t.Parse(texto))
    t.Execute(os.Stdout, nil)
```

// Resultado -> hello Mary!

#### De Diseño

```go
const tmpl = `
<span>hello {{gettext .}}</span>
<span>hello {{. | gettext}}</span>
`

var funcMap = template.FuncMap{
    "gettext": gettext,
}

func gettext(s string) string {
  	if s == "world" {
  		  return "otraCosa"
  	}
  	return s
}

func main() {
  	t, _ := template.New("foo").Funcs(funcMap).Parse(tmpl)
  	s := "world"
  	t.Execute(os.Stdout, s)
}
```

### Must

Es una funcion del paquete template para validar plantillas

### Nested templates

* Se puede declarar una plantilla:  
> {{ define "sub-plantilla"}}   
>   contenido que sea  
> {{ end}}

* Luego esa plantilla se inserta
> {{ template "sub-plantilla" }}  

base.html

```html
{{define "base"}}
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>{{template "title" .}}</title>
</head>
<body>
{{template "content" .}}
</body>
</html>
{{end}}
```

index.html

```html
{{template "base" .}}
{{define "title"}}my title{{end}}
{{define "content"}}
<div>hello {{.}}</div>
{{end}}
```

nestedTemplates.go

```go
func main() {
  	// cannot put base.html before index.html will give empty output
   	// t, _ := template.ParseFiles("base.html", "index.html")
  	t, _ := template.ParseFiles("index.html", "base.html")
  	name := "world"
  	t.Execute(os.Stdout, name)
}
```

## ERRORS

[Errores en Go](https://blog.golang.org/error-handling-and-go)

Los captura un tipo interfaz predefinido cuyo unico metodo `Error` devuelve
una cadena  

```go
type error interface {
    Error() string
}
```

Forma estandard de tratar los errores

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

## FLAG

`import "flag"`  

Para enviar argumentos a un comando  

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

---

## HASH/CRC32

`import hash/crc32`    


1B-85

---

## CRYPTO/SHA1

`import crypto/sha1`    


1B-86
