# GOLANG PARA DESARROLLO WEB  

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

---

## NET/URL

[URL parsing](https://gobyexample.com/url-parsing)  

---