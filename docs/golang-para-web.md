# GOLANG WEB DEV

---

## NET/HTTP

`import net/http`    

### Static

```go
// serves the entire directory  
func main() {		
	dir := http.Dir("./files")
	http.ListenAndServe(":8080", http.FileServer(dir))
	http.HandleFunc("/", readme)
}
```

```go
// ServeFile serves a file or directory as a 3rd argument  
func main() {
	http.HandleFunc("/", public)
	http.ListenAndServe(":8080", nil)
}
func public(w http.ResponseWriter, r *http.Request) {
	http.ServeFile(w, r, "./files/hola.html")
	//	http.ServeFile(w, r, "./files/")
}
```

```go
// serve the directory ./files in the path / static /
// ./files can be anywhere in the file system
// not only in the dir of the application  
func main() {
	dir := http.Dir("./files/")
	handler := http.StripPrefix("/static/", http.FileServer(dir))
	http.Handle("/static/", handler)

	http.HandleFunc("/", homePage)
	http.ListenAndServe(":8080", nil)
}
```

### Handler

\ - Handlers are any struct that has a method `ServeHTTP (w  http.ResponseWriter, r * http.Request)` with two parameters: an HTTPResponseWriter interface and a pointer to a Request struct.  
\ - Handler functions are functions that behave like handlers. They have the same signature as the ServeHTTP method and are used to process requests (Requests)  
\ - Handlers and handler functions can be linked to allow the processing of parts of requests through the separation of issues.  
\ - Multiplexers (routers) are also handlers. ServeMux is a router for HTTP requests. Accepts HTTP requests and redirects them to the appropriate handler according to the URL of the request.  
`DefaultServeMux` is an instance of` ServeMux` that is used as the default router
 

* **Handler**

```go
// multi handler and chain handler
package main

import (
	"fmt"
	"net/http"
)

type helloHandler struct{}

func (h *helloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request){
	fmt.Fprintf(w, "Hello!")
}

type worldHandler struct{}

func (h *worldHandler) ServeHTTP(w http.ResponseWriter, r *http.Request){
	fmt.Fprintf(w, "World!")
}

func log(h http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request){
		fmt.Printf("Handler called - %T\n", h)
		h.ServeHTTP(w, r)
	})
}

func protect(h http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request){
		// some code to make sure the user is authorized
		h.ServeHTTP(w, r)
	})
}

func main() {
	hello := helloHandler{}
	world := worldHandler{}

	server := http.Server{
		Addr: "127.0.0.1:8080",
	}

	http.Handle("/hello", protect(log(&hello)))
	http.Handle("/world", &world)

	server.ListenAndServe()
}
```

* **HandleFunc**

```go 
package main

import (
	"fmt"
	"net/http"
	"reflect"
	"runtime"
)

func hello(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello!")
}

func world(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "World!")
}

func log(h http.HandlerFunc) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		name := runtime.FuncForPC(reflect.ValueOf(h).Pointer()).Name()
		fmt.Println("Handler function called - " + name)
		h(w, r)
	}
}

func main() {
	server := http.Server{
		Addr: "127.0.0.1:8080",
	}
	http.HandleFunc("/hello", log(hello))
	http.HandleFunc("/world", world)

	server.ListenAndServe()
}
```

### Request

* **URL**

```go
https://golang.org/src/net/url/url.go
type URL struct {
	Scheme     string
	Opaque     string    // encoded opaque data
	User       *Userinfo // username and password information
	Host       string    // host or host:port
	Path       string
	RawPath    string // encoded path hint 
	ForceQuery bool   // append a query ('?') even if RawQuery is empty
	RawQuery   string // encoded query values, without '?'
	Fragment   string // fragment for references, without '#'
}
algunos metodos
// EscapedPath returns the escaped form of u.Path.
func (u *URL) EscapedPath() string {}
// IsAbs reports whether the URL is absolute.
func (u *URL) IsAbs() bool {}
// Query parses RawQuery and returns the corresponding values.
func (u *URL) Query() Values {}
type Values map[string][]string
```

* **Headers**

`type Header`  
`func (h Header) Add(key, value string)`  
`func (h Header) Del(key string)`  
`func (h Header) Get(key string) string`  
`func (h Header) Set(key, value string)`  
`func (h Header) Write(w io.Writer) error`  
`func (h Header) WriteSubset(w io.Writer, exclude map[string]bool) error`  

```go
func headers(w http.ResponseWriter, r *http.Request) {
	h := r.Header
	// h := r.Header["Accept-Encoding"]  // devuelve un map de strings
	// h := r.Header.Get("Accept-Encoding") // devuelve string
	fmt.Fprintln(w, h)
}
http.HandleFunc("/headers", headers)
```


* **Body**

```go

func body(w http.ResponseWriter, r *http.Request) {
	len := r.ContentLength
	body := make([]byte, len)
	r.Body.Read(body)
	fmt.Fprintln(w, string(body))
}
	http.HandleFunc("/body", body)
```

### ResponseWriter

`ResponseWriter` interface has three methods:  
\ - `Write` - take a [] bytes and write it in the body of the HTTP response. If the header does not specify content-type it uses the first 512 bytes of data to detect the content type  
\ - `WriteHeader` - sends an integer that represents the status code of the HTTP response. After using this method you can not write or modify anything in the header. If this default method is not used when calling `Write` the code` 200 OK` is sent. It is very useful to send error codes  
\ - `Header` - returns a map of fields in the header that can be modified and sent in the response to the client  

```go
type post struct {
	User    string
	Langs []string
}

func writeExample(w http.ResponseWriter, r *http.Request) {
	str := `<html>
<head><title>Write Example</title></head>
<body><h1>Hello World</h1></body>
</html>`
	w.Write([]byte(str))
}

func writeHeaderExample(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(501)
	fmt.Fprintln(w, "Not implemented yet")
}

func headerExample(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Location", "https://jolav.me")
	w.WriteHeader(302)
}

func jsonExample(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "application/json")
	post := &post{
		User:    "jolav",
		Langs: []string{"Go", "HTML", "Javascript"},
	}
	json, _ := json.Marshal(post)
	w.Write(json)
}

func main() {
	server := http.Server{
		Addr: "127.0.0.1:8080",
	}
	http.HandleFunc("/write", writeExample)
	http.HandleFunc("/writeheader", writeHeaderExample)
	http.HandleFunc("/redirect", headerExample)
	http.HandleFunc("/json", jsonExample)
	server.ListenAndServe()
}
```

### Middleware

[Middleware](https://www.google.es/search?q=middleware+golang&ie=utf-8&oe=utf-8&client=firefox-b&gfe_rd=cr&ei=a4uGWMuFEems8wfuxYi4Dw)  

```go
type Middleware []http.Handler

// Adds a handler to the middleware
func (m *Middleware) Add(handler http.Handler) {
    *m = append(*m, handler)
}
func (m Middleware) ServeHTTP(w http.ResponseWriter, r *http. ➥Request) {
  // Process the middleware
}
```

### Cookies

```go
https://golang.org/src/net/http/cookie.go

type Cookie struct {
	Name  			string
	Value 			string
	Path     	  string    // optional
	Domain      string    // optional
	Expires     time.Time // optional
	RawExpires  string    // for reading cookies only
	// MaxAge=0 means no 'Max-Age' attribute specified.
	// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
	// MaxAge>0 means Max-Age attribute present and given in seconds
	MaxAge   		int
	Secure   		bool
	HttpOnly 		bool
	Raw      		string
	Unparsed 		[]string // Raw text of unparsed attribute-value pairs
}
```

If the `Expires` field is not used, the cookie is session or temporary and is deleted from the browser when it is closed. Otherwise the cookie is persistent and lasts until it expires or is deleted. Use `MaxAge` instead of` Expires` which is deprecated  

```go
package main

import (
	"encoding/base64"
	"fmt"
	"net/http"
	"time"
)

func setCookie(w http.ResponseWriter, r *http.Request) {
	c1 := http.Cookie{
		Name:     "first cookie",
		Value:    "first cookie value",
		HttpOnly: true,
	}
	c2 := http.Cookie{
		Name:     "second cookie",
		Value:    "second cookie value",
		HttpOnly: true,
	}
	http.SetCookie(w, &c1)
	http.SetCookie(w, &c2)
}

func getCookie(w http.ResponseWriter, r *http.Request) {
	c1, err := r.Cookie("first cookie")
	if err != nil {
		fmt.Fprintln(w, "Cannot get the first cookie")
	}
	cs := r.Cookies()
	fmt.Fprintln(w, c1)
	fmt.Fprintln(w, cs)
}

func setMessage(w http.ResponseWriter, r *http.Request) {
	msg := []byte("Hello World!")
	c := http.Cookie{
		Name:  "flash",
		Value: base64.URLEncoding.EncodeToString(msg),
	}
	http.SetCookie(w, &c)
}

func showMessage(w http.ResponseWriter, r *http.Request) {
	c, err := r.Cookie("flash")
	if err != nil {
		if err == http.ErrNoCookie {
			fmt.Fprintln(w, "No message found")
		}
	} else {
		rc := http.Cookie{
			Name:    "flash",
			MaxAge:  -1,
			Expires: time.Unix(1, 0),
		}
		http.SetCookie(w, &rc)
		val, _ := base64.URLEncoding.DecodeString(c.Value)
		fmt.Fprintln(w, string(val))
	}
}

func main() {
	server := http.Server{
		Addr: "127.0.0.1:8080",
	}
	http.HandleFunc("/setCookie", setCookie)
	http.HandleFunc("/getCookie", getCookie)
	http.HandleFunc("/setMessage", setMessage)
	http.HandleFunc("/showMessage", showMessage)
	server.ListenAndServe()
}
```

### Sessions

[Astaxie Sessions and Cookies](https://astaxie.gitbooks.io/build-web-application-with-golang/content/en/06.0.html)

### Forms

1st - Parse request with `ParseForm or ParseMultipartForm`
2nd - We access the form

```html  
// process1 form  
<form action="http://127.0.0.1:8080/process1?hello=world&thread=123" 
method="post" enctype="application/x-www-form-urlencoded">
	<input type="text" name="hello" value="jolav" />
	<input type="text" name="post" value="1234" />
	<input type="submit" />
</form>
<!--
We would get map [thread: [123] hello: [jolav world] post: [1234]]
We have the values of the URL plus the ones in the form
To get only one field we use notation r.Form ["post"]

If we use r.PostForm, the URL pairs are ignored and only the
of the form resulting map [post: [1234] hello: [jolav]]

There is also ParseMultipartForm 
-->
// process2 and process3 form  
<form action="http://localhost:8080/process?hello=world&thread=123"
method="post" enctype="multipart/form-data">
	<input type="text" name="hello" value="jolav" />
	<input type="text" name="post" value="1234" />
	<input type="file" name="uploaded">
	<input type="submit">
</form>
```

```go
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func process1(w http.ResponseWriter, r *http.Request) {
	r.ParseForm()
	fmt.Fprintln(w, r.Form["campo"][0])
	fmt.Prinltln(w, r.Form.Get("campo"))
	// 	fmt.Fprintln(w, r.PostForm)
}

func process2(w http.ResponseWriter, r *http.Request) {
	file, _, err := r.FormFile("uploaded")
	if err == nil {
		data, err := ioutil.ReadAll(file)
		if err == nil {
			fmt.Fprintln(w, string(data))
		}
	}
}

func process3(w http.ResponseWriter, r *http.Request) {
	r.ParseMultipartForm(1024)
	fileHeader := r.MultipartForm.File["uploaded"][0]
	file, err := fileHeader.Open()
	if err == nil {
		data, err := ioutil.ReadAll(file)
		if err == nil {
			fmt.Fprintln(w, string(data))
		}
	}
}

func main() {
	server := http.Server{
		Addr: "127.0.0.1:8080",
	}
	http.HandleFunc("/process1", process1)
	http.HandleFunc("/process2", process2)
	http.HandleFunc("/process3", process3)

	server.ListenAndServe()
}
```

![gowebdev](/_img/go/forms.png)

### Cliente HTTP


`type Client`  
`func (c *Client) Do(req *Request) (*Response, error)`  
`func (c *Client) Get(url string) (resp *Response, err error)`  
`func (c *Client) Head(url string) (resp *Response, err error)`  
`func (c *Client) Post(url string, bodyType string, body io.Reader) (resp *Response, err error)`  
`func (c *Client) PostForm(url string, data url.Values) (resp *Response, err error)`  

Example :http requests `get`  

Put it in the main or wherever it is to make sure it has a maximum waiting time and it does not stay hung waiting to infinity (which is the default value)  
[Reddit Link](https://reddit.com/r/golang/comments/45mzie/dont_use_gos_default_http_client/)

```go
http.DefaultClient.Timeout = 10 * time.Second
```

```go
func getHttpRequest() {
	url := "https://codetabs.com/github-stars/github-star-history.html" 
	resp, err := http.Get(url)
	if err != nil {
		log.Fatal(err)
	}
	defer resp.Body.Close()
	decoder := json.NewDecoder(resp.Body)
	err = decoder.Decode(&geo)
	if err != nil {
		panic(err)
	}
	aux.SendDataToClient(w, r, geo)
}
```  

### ServeMux

```go
func mainNormal() {
	// assets for all apps
	assets := http.FileServer(http.Dir("_public"))
	http.Handle("/", http.StripPrefix("/", assets))

	// assets for individual apps
	votingRes := http.FileServer(http.Dir("voting/assets"))
	http.Handle("/voting/assets/", 
		http.StripPrefix("/voting/assets/", votingRes))

	book := http.FileServer(http.Dir("./book/"))
	nightlife := http.FileServer(http.Dir("./nightlife/"))
	stock := http.FileServer(http.Dir("./stock/"))

	http.Handle("/book/", http.StripPrefix("/book", book))
	http.Handle("/nightlife/", http.StripPrefix("/nightlife", nightlife))
	http.Handle("/stock/", http.StripPrefix("/stock", stock))

	// any /voting/* will redirect to voting.Voting
	http.HandleFunc("/voting/", voting.Router)

	// any /pintelest/* will redirect to voting.Voting
	http.HandleFunc("/pintelest/", nodePintelest)

	server := http.Server{
		Addr: "localhost:3006",
	}
	server.ListenAndServe()
}
```

```go
func mainMux() {
	mux := http.NewServeMux()

	// assets for all apps
	assets := http.FileServer(http.Dir("_public"))
	mux.Handle("/", http.StripPrefix("/", assets))

	// assets for individual apps
	votingRes := http.FileServer(http.Dir("voting/assets"))
	mux.Handle("/voting/assets/", 
		http.StripPrefix("/voting/assets/", votingRes))

	book := http.FileServer(http.Dir("./book/"))
	nightlife := http.FileServer(http.Dir("./nightlife/"))
	stock := http.FileServer(http.Dir("./stock/"))

	mux.Handle("/book/", http.StripPrefix("/book", book))
	mux.Handle("/nightlife/", http.StripPrefix("/nightlife", nightlife))
	mux.Handle("/stock/", http.StripPrefix("/stock", stock))

	mux.HandleFunc("/voting/", voting.Router)
	//mux.HandleFunc("/voting/p/", nodePintelest)

	// any /pintelest/* will redirect to nodePintelest
	mux.HandleFunc("/pintelest/", nodePintelest)

	server := http.Server{
		Addr:    "localhost:3006",
		Handler: mux,
	}
	server.ListenAndServe()
}
```

```go
// http://codepodu.com/subdomains-with-golang/
type Subdomains map[string]http.Handler
func (subdomains Subdomains) ServeHTTP(w http.ResponseWriter, 
							r *http.Request) {
	domainParts := strings.Split(r.Host, ".")
	if mux := subdomains[domainParts[0]]; mux != nil {
		// Let the appropriate mux serve the request
		mux.ServeHTTP(w, r)
	} else {
		// Handle 404
		http.Error(w, "Not found", 404)
	}
}
type Mux struct {
	http.Handler
}
func (mux Mux) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	mux.ServeHTTP(w, r)
}

func adminHandlerOne(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "It's adminHandlerOne , Hello, %q", r.URL.Path[1:])
}
func adminHandlerTwo(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "It's adminHandlerTwo , Hello, %q", r.URL.Path[1:])
}
func analyticsHandlerOne(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "It's analyticsHandlerOne , Hello, %q", r.URL.Path[1:])
}
func analyticsHandlerTwo(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "It's analyticsHandlerTwo , Hello, %q", r.URL.Path[1:])
}

func main() {
	adminMux := http.NewServeMux()
	adminMux.HandleFunc("/admin/pathone", adminHandlerOne)
	adminMux.HandleFunc("/admin/pathtwo", adminHandlerTwo)

	analyticsMux := http.NewServeMux()
	analyticsMux.HandleFunc("/analytics/pathone", analyticsHandlerOne)
	analyticsMux.HandleFunc("/analytics/pathtwo", analyticsHandlerTwo)

	subdomains := make(Subdomains)
	subdomains["admin"] = adminMux
	subdomains["analytics"] = analyticsMux

	http.ListenAndServe(":8080", subdomains)
}
```

---

## HTML/TEMPLATE

`import html/template`

* To use it you have to import the `html/template` package  
* create template `t, _: = template.ParseFiles("index.html")`  
* assign value to template variables template_value: = "Hello"`  
* serve the page `t.Execute(w, template_values)`  

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

* `{{}}` anything to be rendered must go between double parentheses  

* `{{.}}` abbreviation for the current object  

* `{{.FieldName}}` field FieldName of the current object  

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

* {{if}} {{else}} : Only for Boolean values, it does not make comparisons  

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

* {{ . | html}} For example we use this to catch the current object '.' Y
apply escape to HTML to the object  

### Variables

Pass variables to templates

```go
// using anonymous structs
var templates = template.Must(template.ParseGlob("templates/*"))

func handle(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/html; charset=utf-8")
	templates.ExecuteTemplate(w, "page.html", struct {
		PageTitle string
		Message string
		User string
		}{"Template example: struct", "Hello", "World"})
}
```

```go
// using Maps
var templates = template.Must(template.ParseGlob("templates/*"))

func handle(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/html; charset=utf-8")
	m := make(map[string]interface{})
	m["PageTitle"] = "Template example: map"
	m["Message"] = "Hello"
	m["User"] = "World"
	templates.ExecuteTemplate(w, "page.html", m)
}
```

```go
// map
m := map[string]interface{}{
	"imgs": imgs, // {{range .imgs.Image}}{{.}}{{end}}
	"user": p,    //{{.user.Name}}
}

// struct , Images []Image
type Data struct {
	I Images
	P Profile
}
var d Data
d.I = imgs // {{range .I.Image}}{{.}}{{end}}
d.P = p    // {{.P.Name}}*/
t.Execute(w, &m) 
```

### Functions

#### Predefined

For example `print` equals `fmt.Sprintf`

```go
func main() {
    texto := "{{with $x := `hello`}}{{printf `%s %s` $x `Mary`}}
        {{end}}!\n"
    t := template.New("test")
    t = template.Must(t.Parse(texto))
    t.Execute(os.Stdout, nil)
```

// Result -> hello Mary!

#### Custom

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

It is a function of the template package to validate templates  

### Nested templates

* You can declare a template:
> {{define "sub-template"}}  
> content that is  
> {{end}}  

* Then that template is inserted
> {{template "sub-template"}}  

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

### Pass variables to js

Pass golang variables to client js

```html
<!-- guest.html-->
<script type="text/javascript">
  // Pass golang vars to client js
  //window.addEventListener('load', voting.init({{.}}))
  var golang = {{.}} // passes as a global object variable
  //var golang = "{{ .}}"; // as string
  window.addEventListener('load', function () {
    voting.init()
  })
</script>
```

```go
// voting.go
func guest(w http.ResponseWriter, r *http.Request) {
	var data aPoll
	data = aPoll{Question: "question text"}
	tmpl["guest.html"].ExecuteTemplate(w, "guest.html", data)
}
```

```javascript
var voting = (function () {
  function init () {
    console.log("Data from golang", golang);
  }
	return {
    init: init
  };
}());
```

---

## NET/URL

### Add parameters to URL

```go
// existing URL
values := r.URL.Query()
values.Add("newParameterName", value)
values.Get("valueName", value)
r.URL.RawQuery = values.Encode()
fmt.Println(r.URL.String())
fmt.Println(values["newParemeterName"])

// new URL
urlData, err := url.Parse("https://anapi.com/custom/v1?q=")
params := url.Values{}
params.Add("q", r.URL.Query().Get("q"))
params.Add("cx", c.APIImage.CseID)
params.Add("key", c.APIImage.Key)
params.Add("num", r.URL.Query().Get("num"))
params.Add("offset", r.URL.Query().Get("offset"))
urlData.RawQuery = params.Encode()
```

[URL parsing](https://gobyexample.com/url-parsing)  

---

## UTILS

### fresh

[https://github.com/pilu/fresh](https://github.com/pilu/fresh) - like nodemon but for golang

`fresh -c path/to/custom/config` - use with custom config file  

```sh
root:              .
tmp_path:          ./tmp
build_name:        runner-build
build_log:         runner-build-errors.log
valid_ext:         .go, .tpl, .tmpl, .html, .css, .js
ignored:           assets, tmp, pintelest
build_delay:       600
colors:            1
log_color_main:    cyan
log_color_build:   yellow
log_color_runner:  green
log_color_watcher: magenta
log_color_app:
```

---