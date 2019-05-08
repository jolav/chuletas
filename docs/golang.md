# GOLANG 1.12.X

---

## **INSTALL**

[download golang from here](https://golang.org/dl/)

As root  
`tar -C /usr/local -xzf go-file.tar.gz`

```ssh
// as user
// debian $HOME/.profile and $HOME/.bashrc
// mac $HOME/.bash_profile and $HOME/.bashrc

# Golang conf
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/path/to/go/code
export PATH=$PATH:$GOPATH/bin/binLinux

// Next line not sure if it's necessary
export GOBIN=$PATH:$GOPATH/bin/binLinux|binMac
```

```ssh
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/golang
export PATH=$PATH:$GOPATH/bin
```

Reload conf  
`source ~/.profile`   

Folders that are created:  

* `bin` - Contains the compiled binaries. We can add the bin folder to the system path to make compiled binaries executable from anywhere  
* `pkg` - contains the compiled versions of the available libraries so that the compiler can link them without having to recompile them  
* `src` - contains all the code organized by import routes 


**VS Code**

`Go: Install/Update Tools` - then move binaries from bin to binLinux | bin/Mac

---

## **GO TOOL**

```ssh
	go <command> [arguments]

The commands are:

	bug         start a bug report
	build       compile packages and dependencies
	clean       remove object files and cached files
	doc         show documentation for package or symbol
	env         print Go environment information
	fix         update packages to use new APIs
	fmt         gofmt (reformat) package sources
	generate    generate Go files by processing source
	get         download and install packages and dependencies
	install     compile and install packages and dependencies
	list        list packages or modules
	mod         module maintenance
	run         compile and run Go program
	test        test packages
	tool        run specified go tool
	version     print Go version
	vet         report likely mistakes in packages

Use "go help <command>" for more information about a command.

Additional help topics:

	buildmode   build modes
	c           calling between Go and C
	cache       build and test caching
	environment environment variables
	filetype    file types
	go.mod      the go.mod file
	gopath      GOPATH environment variable
	gopath-get  legacy GOPATH go get
	goproxy     module proxy protocol
	importpath  import path syntax
	modules     modules, module versions, and more
	module-get  module-aware go get
	packages    package lists and patterns
	testflag    testing flags
	testfunc    testing functions

Use "go help <topic>" for more information about that topic.
```

* **cross compile**

`GOOS` -  the target operating system  
`GOARCH` - the target processor  

```ssh
GOOS=darwin GOARCH=386 go build
```

---

## OPERATORS  

* **Arithmetic**

> `+` Addition  
> `-` Substraction  
> `*` Multiplication  
> `/` Quotient  
> `%` Remainder   
> `++` +1  
> `--` -1  

* **assignment**

> `=` x = y  
> `+=` x = x + y  
> `-=` x = x - y  
> `*=` x = x * y  
> `/=` x = x / y  
> `%=` x = x % y  

* **Comparison**

> `==` equal  
> `!=` not equal    
> `>` greater than    
> `<` less than  
> `>=` greater than or equal  
> `<=` less than or equal    

* **Logical**

> `&&` AND  
> `||` OR  
> `!` NOT  

* **Pointers**

> `&` returns address of (create id no exists) pointer 
> `*` deference pointer(returns value of pointer address)  

---

## VARIABLES

A variable can contain any type, even a function

```go
func main() {
    accion := func() {
        fmt.Println("Hola")
    }
    accion()
}
```

TypeOf(variable)

```go
import ("reflect")
fmt.Println("....", reflect.TypeOf(variable))
```

### Declaration

* **declaration**

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

const constant ="i am a constant"
```

* **initialization**

```go
var (
    name      string  = "jolav"
    age       int     = 100
)
var (  // inferred typing
    name = "jolav"
    age  = 32
)
var name, location, age = "jolav", "home", 100
```

* **shorthand `:=`**   

only in func bodies, implicit type

```go
func main() {
    name, location := "jolav", "home"
    age := 100
}
```

* **new**

Reset the type value to zero and return a pointer to it.

```go
x := new(int)
```

* **make**

Necessary for `slices`` maps` and `channels`

* **Zero Values**

When variables are declared without an explicit value they are assigned a zero value

> `int` - 0  
> `float` - 0.0   
> `string` - ""   
> `boolean` - false  
> `pointers` - nil  
> `map` - nil  
> `slices` - nil  
> `array` - array ready to use with its elements at zero value.  
> `functions` - nil
> `interfaces` - nil  
> `channels` -nil  

* **type**

```go
package tempconv

import "fmt"

type Celsius float64
type Fahrenheit float64

const (
    AbsoluteZeroC   Celsius = -273.15
    FreezingC       Celsius = 0
    BoilingC        Celsius = 100
)

func CToF(c Celsius) Fahrenheit { 
    return Fahrenheit(c*9/5 + 32) 
    }
func FToC(f Fahrenheit) Celsius { 
    return Celsius((f - 32) * 5 / 9) 
}
```

### Scope

The scope is the zone of the program where a defined variable exists  

Types of variables according to where they are declared:

* `local variables` - within a function or a block. Outside of that environment
they do not exist
* `package variables` - out of all functions or blocks. Accessible from
any part of the package
* `formal parameters` - in the definition of the parameters of a function.
they are treated as locals for that function and have a preference over global ones

When they coincide within a function or block a local and a global prevails
the local

### Type conversions

Go has no implicit conversion of types  
`T(v)` - Convert the value `v` to the` T` type

```go
i := 42
f := float64(i)
u := uint(f)
```

* **strconv**


* **Type Assertion**

```go
func diffArray(s1, s2 interface{}) []string {
	var aux1 []int
   	fmt.Println(reflect.TypeOf(s1))
	var a1, a2 []string
	if reflect.TypeOf(s1) == reflect.TypeOf(aux1) { // s1,s2 son []int
		a1, a2 = convertIntToString(s1.([]int), s2.([]int))
        // pass s1,s2 as []int and then use type assertion
	} else {
		a1, a2 = s1.([]string), s2.([]string)
	}
	// now a1,a2 are []string

func diffTwoArrays() {
	diffArray([]int{1, 2, 3, 5}, []int{1, 2, 3, 4, 5}))
	diffArray([]string{"diorite", "andesite", "grass", "dirt", 
    "pink wool", "dead shrub"}, 
    []string{"diorite", "andesite", "grass", "dirt", "dead shrub"})
}
```

### Pointers

* **Pointers vs value**  

Everything in Go is passed for value, but ...  
When a variable of reference type is declared, a value called `header value` is created, which contains a pointer to the underlying data structure needed for each reference type.  
Each reference type contains unique fields to manage the underlying data structure itself.  
The `header value` contains a pointer, therefore you can pass a copy of any type of reference and share the underlying structure intrinsically when sharing the pointer.  

`int` - value  
`float` - value  
`string` - variable of reference type, but works as a value  
`boolean` - value  
`arrays` - value  
`slices` - reference type variable  
`maps` - variable of reference type  
`functions` - reference type variable  
`interfaces` - variable of reference type  
`channels` - reference type variable  

* ** Pointers **

By default Go passes arguments by value (creates a copy)  
To pass them by reference you have to pass pointers or use structures of
Data that uses values ​​by reference such as slices and maps.  

`&` - to get the pointer of a value we put it in front of its name  
`*` - to dereference a pointer and give us access to its value  

If `p` is a pointer to `x`  
`&x` -> `p = & x` p is the pointer of x (contains the memory address of x)  
`*p` -> `* p = x` * p is the value of x  


```go
i := 42
p := &i             // generates a pointer to i
fmt.Println(*p)     // read i through the pointer p
*p = 21             // set i through the pointer p
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

```go
func  main()  {
  var i int = 7
  var p *int
  p =  &i

  fmt.Println("i : " , i)
  fmt.Println("memory address of i : ", &i)
  fmt.Println("p : " , p)
  fmt.Println("*p : " , *p)
}
[output]
i :  7
memory address of i :  0x10328000
p :  0x10328000
*p :  7
```

* **new**

`new` - take a type as an argument, allocate enough memory for that type
of data and returns a pointer that points to that memory. Then the garbage
collector cleans everything

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

* **Mutability**

Only the constants are immutable.
However, as arguments are passed by value, a function that receives and
modify an argument does not change the original value

* Example

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
// with pointers
func addOne(x *int) {
	*x++
}
func main() {
	x := 0
	addOne(&x)
	fmt.Println(x)          // x da 1
}
```

[stackoverflow pointer vs values](https://stackoverflow.com/questions/23542989/pointers-vs-values-in-parameters-and-return-values)   

```go
type data struct {
    val int
}

func myfunc() data {
    // returns a copy of the struct
    return data{val: 1}  
}

func myfunc() *data {
    // returns a pointer to the struct created within the function
    return &data{}
}

func myfunc(d *data) {
    // receives an existing struct and overwrites its value
    d.val = 1
}
```

---

## BASIC TYPES


### Numbers

![go](./_img/go/basicTypes.png)


### Booleans

`&&` - and  
`||` - or   
`!` - not  

> ![go](./_img/go/boolean1.png)
> ![go](./_img/go/boolean2.png)
> ![go](./_img/go/boolean3.png)


### Strings

They are made of bytes (one per character)  
The difference between single and double quotes is that doubles can not contain
new lines and are allowed to escape special characters  

`len (string)` - length of the string  
`"Hello world "[1]` - access string characters  
`"Hello, "+ World"`   

### Constantes

They are declared as variables but with the keyword `const`.  
They can not be declared using `: =`  
They can only be characters, string, boolean or numeric values.  

```go
const PI = 3.14
```

### Iota

[iota info](https://splice.com/blog/iota-elegant-constants-golang/)

It is an identifier used in constant declarations to indicate that they are
autoincrementables.  
It resets to zero when the reserved word `const` appears

```go
const ( // iota is reset to 0
    c0 = iota  // c0 == 0
    c1 = iota  // c1 == 1
    c2 = iota  // c2 == 2
)
```

---

## CONTROL STRUCTURES

### for

```go
// standard for
sum := 0
for i := 0; i < 10; i++ {
  sum = sum + i
}

// for wihout pre/post declarations
sum := 1
for ; sum < 1000; {
  sum = sum + sum
}

// for (like a while)
sum := 1
for sum < 1000 {
  sum = sum + sum
}

// for infinite
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

> `switch` 

* Only values of the same type can be compared
* declaration `default` to execute if all others fail
* in the statement you can use an expression (pej calculate a value)
`case 3 - 2:`
* You can have multiple values only one case
`case 6, 7:`
* `fallthroguh` all statements that meet the condition are executed
* `break` comes out of the switch

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


* **slice**

loops over `slices` or `maps` 

```go
var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
func main() {
    for i, v := range pow {
    		fmt.Println("Posicion", i, "valor", v)
    }
}
```

We can omit the index using `_`  
We can omit the value by omitting completely `, value`

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

The first parameter is not an autoincrementable integer but the key of the map  
`for key, value := range cities`

* **break**

Stop loop as desired

* **continue**

Ypu can skip one iteration

---

## ARRAYS

**`[n]T` - is an array of `n` elements of type `T`**    

* Cant resize 
* can be initialized when declaring them  
    `a := [2]string{"hello", "world!"}`  
    `a := [...]string{"hello", "world!"}` using an ellipsis to indicate a number a variable number of elements that in this case are two  
    `a := [5]int{1: 10, 2: 20}` - initializing only some values   
* `printìng arrays`  
    `fmt.Printf("%q\n", a)    // ["hello" "world!"]`
* `len(array)`  
* Multidimensional    
    `var a [4][2]int`  
    `array := [4][2]int{{10, 11}, {20, 21}, {30, 31}, {40, 41}}`      


---

## SLICES

**`tipo []T` - is an slice of type `T` elements**  

* **create slice**  

Must be created before they can be used

> * `slice literal`    
`mySlice := []int{2, 3, 5, 7, 11, 13}`

> * `make` - creates an empty slice with a length and (optional a capacity)    
`cities := make([]string, len, cap)`  

* **Cut slice**

`s[a:b]` - selects elements from pos a (inclusive) to b (exclusive)   
`s[:b]` - an index `a` that is not declared is an implicit `0`  
`s[a:]` - an `b` index that is not declared is implicit a `len (s)`    

* **Append slice**

`cities = append(cities, "San Diego")`  
`cities = append(cities, "San Diego", "Mountain View")`  
`otherCities := []string{"Santa Monica", "Venice"}`  
`cities = append(cities, otherCities...)`  

* **Copy slice**

`copy(destination, source)`  

* **Length**

`len(slice)`  

* **Nil slices**

Declaration   
`var z []int` -The zero value of a slice is nil. A slice nil has a zero length  
Initialization  
`z := make([]int, 0)`  
`z := []int{}`  

Three ways are the same

* **BiDimensional**

```go
// allocate composed 2d array
a := make([][]int, row)
for i := range a {
	a[i] = make([]int, col)
}

// allocate composed 2d array
a := make([][]int, row)
e := make([]int, row * col)
for i := range a {
	a[i] = e[i*col:(i+1)*col]
}

// another way
func get(r, c int) int {
	return e[r*cols+c]
}
```

---

## MAPS

**`tipo map[a]b - is a map which keys are type a and values type b`**   

`key / value pairs`

* **Crete map :**  

Map must be created before using them

> * `map literal`    
`friends := map[string]int{"John":50, "Helen":21, "Charles":41,}`
> * `make` - creates nil map empty    
`friends := make(map[string]int)`  

If we declare it but we do not initialize it, when we try to add elements it will not compile   

> * `friends := map[string]int{}` - declared but not initialized    

* **Modifying maps**

`m[key] = elem` - Insert or update  
`elem = m[key]` - returns element  
`delete(m, key)` - deletes element   
`elem, ok = m[key]` - check if there is a value with a certain key  

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

It is a collection of fields / properties  
Only the exported fields (first capital letter) are accessible outside the package   

### Inicializacion

```go
type Circle struct {
    x, y, r float64
}
```

`var c Circle` - creates a local variable named circle that defaults values to zero (0 for int, 0.0 for float. "" for string, nil for pointers)  
`c := new(Circle)` - allocates memory for all fields, initializes them to
zero and returns a pointer to the struct (*Circle), pointers are used a lot in
structs so that the functions can modify the data.  
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

```go
// all in one
var addCases = []struct {
	in   string
	want string
}{
	{
		"2011-04-25",
		"2043-01-01T01:46:40",
	},
	{
		"1977-06-13",
		"2009-02-19T01:46:40",
	},
}

type addCases2 []struct {
	in   string
	want string
}
ac := addCases2{
    {
        "2011-04-25",
        "2043-01-01T01:46:40",
    },
    {
        "1977-06-13",
        "2009-02-19T01:46:40",
    },
}

// show them 
for i, v := range addCases {
    fmt.Println(i, v.in)
}
for i, v := range ac {
    fmt.Println(i, v)
}
```

```go
func show() {
	fmt.Println(t[0].hola)
	fmt.Println(test2[1].hola2)
}

type test []struct {
	hola string
}

var t = test{
	{"prueba1"},
	{"prueba2"},
}

var test2 = []struct {
	hola2 string
}{
	{"prueba3"},
	{"prueba4"},
}
```

### Methods

A method is a function with the first implicit argument called a receiver.  
`func (ReceiverType r) func_name (parameters) (results)`

Receiver of the method is between the keyword `function` and the name of the method

`func (u User) Greeting() string` - allows us to call it with u.Greeting()`

* **Code organization**

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

* **Aliases**

To define methods in a type that is not yours, aliases are used  

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

* **Use pointers in the receivers**

The methods can be associated with a name or a pointer. Advantages of using
pointers: 

* avoid copying the value with each call to the method (pass it by reference)  
* be able to modify the value that we passed

```go
type User struct {
    name    string
    email   string
}

func (u user) notify() {
    fmt.Printf("Mandar correo a %s<%s>\n", u.name, u.email)
}

// sin el puntero del receptor el correo no se cambiaria.
func (u *user) changeEmail(email string) {
    u.email = email
}
```

```
TIP
After declaring a new type, try to answer this question before 
declaring methods for the type: 
Does adding or removing something from a value of this type need to
create a new value or mutate the existing one? 
- If the answer is create a new value, then use value receivers for 
your methods. 
- If the answer is mutate the value, then use pointer receivers. 

This also applies to how values of this type should be passed to 
other parts of your program. It’s important to be consistent. 
The idea is to not focus on what the method is doing with the value,
but to focus on what the nature of the value is.
```

### Composition

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

How we access User Struct:    
`a := new(Player)`  
`a.User.Name`  
`a.Name`

---

## INTERFACES

[Interfaces explanation](http://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go)

[More interfaces explanation](https://go-book.appspot.com/interfaces.html) 

[More about interfaces](https://www.goinggo.net/2014/05/methods-interfaces-and-embedded-types.html)   

* It is a set of methods  
* It is a type of data

```go
package main

import "fmt"

type cat struct {
	name string
}

func (c *cat) born() {
	fmt.Println(c.name, "is born Miaouu")
}

type dog struct {
	name string
}

func (d *dog) born() {
	fmt.Println(d.name, "is born Wharff")
}

type animal interface {
	born()
}

func born(a animal) {
	a.born()
}

func main() {
	Jasper := &cat{"JASPER"}
	Lucy := &dog{"Lucy"}
	Max := new(dog)
	Max.name = "Max"
	Max.born()
	// call born function
	born(Jasper)
	born(Lucy)
	born(Max)
}
```

```go
package main

import "fmt"

type Human struct {
	name  string
	age   int
	phone string
}

type Student struct {
	Human  //an anonymous field of type Human
	school string
	loan   float32
}

// A human likes to stay... err... *say* hi
func (h *Human) SayHi() {
	fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

// A human can sing a song, preferrably to a familiar tune!
func (h *Human) Sing(lyrics string) {
	fmt.Println("La la, la la la, la la la la la...", lyrics)
}

// A Human man likes to guzzle his beer!
func (h *Human) Guzzle(beerStein string) {
	fmt.Println("Guzzle Guzzle Guzzle...", beerStein)
}

// A Student borrows some money
func (s *Student) BorrowMoney(amount float32) {
	s.loan += amount // (again and again and...)
}

func Prestar(y YoungChap, amount float32) {
	y.BorrowMoney(amount)

}

// INTERFACES
type Men interface {
	SayHi()
	Sing(lyrics string)
	Guzzle(beerStein string)
}

type YoungChap interface {
	SayHi()
	Sing(song string)
	BorrowMoney(amount float32)
}

func main() {
	mike := Student{Human{"Mike", 25, "222-222-XXX"}, "MIT", 150.50}
	mike.BorrowMoney(10)
	mike.BorrowMoney(10)
	Prestar(&mike, 100)
	fmt.Println("Debe ..", mike.loan)
}
```

* **empty interfaces** 

1- Everything has a `type`, you can define a new `type` for Example `T` that has three methods `A`, `B` and `C`  
2- The set of specific methods of a `type` is called `interface type`. In our Example `T_interface = (A, B, C)`  
3- You can create a new `interface type` by defining the methods it has. Pro Example I think `MyInterface = (A)`  
4- When you specify a variable of type `interface type` you can assign only the types that are in an interface that is a superset of your interface, let all the methods of `MyInterface` must be in `T_interface`  

Conclusion: All types of variables satisfy the `empty interface`. Therefore a function that has an `interface{}` as an argument admits any value whatsoever.  
But within the function the Go runtime converts that value to an `interface{}`

```go
func DoSomething(v interface{}) {
   // la funcion acepta cualquier valor una vez dentro
   // v es del tipo interface{}
}
```

The value of an interface are two `word` of data:  
 - a `word` is a pointer to a table of methods for the value of the underlying `type`  
 - the other `word` is a pointer to the current data of that value  

---

## FUNCTIONS

* **Call Stack**

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

>>> ![go](./_img/go/callStack.png)

* **Arguments**

* The functions can receive 0 or more arguments all typed after the name of the variable.

```go
func add(x int, y int) int {
    return x + y
}
func add(x, y int) int {   // int affects all parameters(x, y)
    return x + y
}
```

```go
// ... functions that accept a variable number of parameters
func add(args ...int) int {   
    total := 0
    for _, v := range args {
        total += v
    }
    return total
}
func main() {         // we pass the parameters that we want
    fmt.Println(add(1,2,3))
    xs := []int{1,2,3}
    fmt.Println(add(xs...)) // we can also pass a slice
}
```
* Return of parameters, can return any number of them

```go
return region, continente   // returns more than one value
// If the return parameters are named, is valid with only one return
func location(name, city string) (region, continent string) {
    ..code
    return    // returns region and continent
}
```

* **Closures**

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

* **Recursion**

```go
func factorial(x uint) uint {   
    if x == 0 {
        return 1
    }
    return x * factorial(x-1)
}
```

### type function

```go
package main
import "fmt"

type test_int func(int) bool

// isOdd takes an ints and returns a bool set to true if the
// int parameter is odd, or false if not.
// isOdd is of type func(int) bool which is what test_int
// is declared to be.

func isOdd(integer int) bool {
    if integer%2 == 0 {
        return false
    }
    return true
}

// Same comment for isEven
func isEven(integer int) bool {
    if integer%2 == 0 {
        return true
    }
    return false
}

// We could've written:
// func filter(slice []int, f func(int) bool) []int
func filter(slice []int, f test_int) []int {
    var result []int
    for _, value := range slice {
        if f(value) {
            result = append(result, value)
        }
    }
    return result
}

func main(){
    slice := []int {1, 2, 3, 4, 5, 7}
    fmt.Println("slice = ", slice)
    odd := filter(slice, isOdd)
    fmt.Println("Odd elements of slice are: ", odd)
    even := filter(slice, isEven)
    fmt.Println("Even elements of slice are: ", even)
}

```

### defer

Defer the execution of a function until the function in which it is

```go
func main() {
    defer fmt.Println("world")
    fmt.Println("hello")
}
```

It is used to free resources when possible

```go
f, _ := os.Open(filename)
defer f.Close()
```

### panic, recover

`panic("panic value")` - create a runtime error.     
`recover()` - stops the panic and returns the value that was passed with the
call panic  

`panic` usually indicates a programming error or a condition exceptional of which there is no easy way to recover.   

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

`go f(x)` begins the execution of a new goroutine that is a function capable of running concurrently with other functions.  

```go
// without wait, the main program may end before the goroutines
func parallelLetFreq() {
	var wg sync.WaitGroup
	wg.Add(3)   // 3 more goroutines we have to wait for
	go count("1", &wg)
	go count("2", &wg)
	go count("3", &wg)
	wg.Wait() // wait for all goroutines
}

func count(n int, wg *sync.WaitGroup) {
	defer wg.Done() // when finishing the function finish goroutine
	fmt.Println("Number --> ", n))
}
```

### channels

`channels` - They are a conduit through which you can receive and send data with the operator `<-`  

`ch <- data` - Send data to the channel ch    
`data := <-ch` - Receive information from the ch channel and assign it to data      
`ch := make(chan int)` - Channels have to be created before using them  

By default the shipments and receptions wait until the other side is ready.  
This allows goroutines to synchronize without specific blocks or terms  

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

`ch := make(chan int, 100)` - We put a buffer to the channels indicating their
length as second argument in the `make` to initialize the channel  
Send data even channel with buffer is blocked if the buffer is full  
Receive data from a channel with buffer is blocked if the buffer is empty   

```go
func main() {
	c := make(chan int, 2)
	c <- 1;    c <- 2;   c <- 3
	fmt.Println(<-c);    fmt.Println(<-c);   fmt.Println(<-c)
}       // fatal error: all goroutines are asleep - deadlock!
```

However, the next one would work. When adding the extra value from a
goroutine does not lock the main thread because although the goroutine is called before the channel empties, this will wait until there is space in the channel. 

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

`close(ch)` - Only one sender can close a channel. Send to a closed channel causes a panic. They are not like files that need to be closed. They only close
to warn the receiver that no more data will arrive and to finish the loop
range.  

`v, ok := <-ch` - Sender may close a channel to indicate that nothing else will be sent. Receivers can test when a channel has been closed
assigning a second parameter to the receiving expression `ok` will be false when there are no more values to receive and the channel is closed.

`for i := range ch` - receives channel values until it is closed 

* **Select**

It's like `switch` but with channels

1A - 89  
1B - 103  

---

## PACKAGES

A Go program is made with packages. The programs start executing the
function `main` inside the `main` package.  
By convention, the package name is the last word in the import path.  
The "math/rand" package includes files that start with the sentence `package rand`  

Packages that are not from the standard library are imported using a web URL,but first you have to download them with `go get`

```go
go get github.com/owner/path/to/code
import "github.com/owner/path/to/code"
```

After importing a package we can use the names that it exports (be variables, methods or functions) from outside the package.  
The names exported in Go start with a capital letter  

```go
package main
import ( "fmt"	"math" )
func main() {
	fmt.Println(math.Pi)
	fmt.Println(math.pi)
}
// cannot refer to unexported name math.pi
```

> **Other ways to import packages**

* `ìmport alias "fmt"` - Create an alias of fmt. Now it's alias.something instead of fmt.something  
* `import . "fmt"` -  Allows access to content directly without having to
be preceded by fmt
* `import _ "fmt"` - Remove the compiled warning on that package
if it is not used. Execute it if there are initialization functions (`func init() {}`), rest of the package remains inaccessible.   

### Create packages  

The package names match the folder where they are. This can be
change but it's not worth it.  
By convention, the package name is the last word in the import path.
`~/src/proyectoX/main.go`  

```go
package main
import "fmt"
import "proyectoX/utils"  // path is from src
func main() {
    // utils.Media(xs)
}
```

`~/src/proyectoX/utils/media.go`

```go
package utils
func Media() {
    // code
}
```

### Uninstall packages

`go clean -i path/package...` - theoretically you delete the pkg and bin, the src you have to delete them manually  

### Update

`go get -u all` -  update all packages  

`go get -u full/package/name` -  update one package

---

## EXECUTE

The program starts with the `main` function of the `package main`  

Before, the `init` functions of that file are executed  

Imported packages "_import path/package" cause the compiler to accept an unused packet and also execute the `init` function(s) of that package  

---

## TESTING

The compiler ignores all the files that end in `_test.go`  

`~/src/proyectoX/utis/media_test.go`

```go
package utis

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

## ERRORS

[Errors in Go](https://blog.golang.org/error-handling-and-go)

It captures a predefined interface type whose only method `Error` returns
a chain   

```go
type error interface {
    Error() string
}
```

Standard way to deal with errors.  
`log.Fatal (err)` - send the error to the terminal and stop the program

```go
f, err := os.Open("filename.ext")
if err != nil {
    log.Fatal(err)
}
```

Less boilerplate  

```go
func check(e error) {
    if e != nil {
        panic(e)
    }
}
// now 
check(err)
```

---

## LINKS

* **Go**

<code>[Lets learn Go!](https://go-book.appspot.com/index.html)</code>  
<code>[Interfaces](http://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go)</code>  
<code>[Tipos de funcion](http://jordanorelli.com/post/42369331748/function-types-in-go-golang)</code>  
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>

* **Blogs**

<code>[Jacob Martin](https://jacobmartins.com/article-list/)</code> - Articles and tutorials  
<code>[Dinosaurscode](https://dinosaurscode.xyz/posts/)</code>  
<code>[thepolyglotdeveloper.com](https://www.thepolyglotdeveloper.com/category/golang/)</code> - Articles and tutorials
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>

* **Web Dev**

<code>[Writing Web Applications](https://golang.org/doc/articles/wiki/)</code> - Basic tutorial of the golang.org wiki  
<code>[How I Start](https://howistart.org/posts/go/1)</code> - Tutorial with multiple weather APIs

<code>[Build Web Application with Golang](https://astaxie.gitbooks.io/build-web-application-with-golang/content/en/)</code> - Book     
<code>[webapp-with-golang-anti-textbook](https://thewhitetulip.gitbooks.io/webapp-with-golang-anti-textbook/content/)</code> - Book  
<code>[Go Web Applications](https://waimengmoan.gitbooks.io/go-applications/content/)</code> - Book 

<code>[Anatomy of a Go Web Application](http://tech.townsourced.com/post/anatomy-of-a-go-web-app/)</code> - 
Structure of a basic web application    
<code>[Anatomy of a Go Web Application](http://tech.townsourced.com/post/anatomy-of-a-go-web-app-authentication/)</code> - Authentication  
<code>[Go Web App Example - Entry Point, File Structure, Models, and Routes](http://www.josephspurrier.com/go-web-app-example/)</code> - Structure of a basic web application     
<code>[Blue Jay - Go Toolkit for the Web](https://blue-jay.github.io/)</code> - Structure of a complex web application  
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>

* **Libraries**

<code>[database/sql](http://go-database-sql.org/index.html)</code>  
<code>[go-sql-driver/mysql](https://github.com/go-sql-driver/mysql)</code>  
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>


* **Deploy security**

<code>[Quick security wins in Golang (Part 1)](https://blog.komand.com/quick-security-wins-in-golang)</code>  
<code>[https://github.com/unrolled/secure](https://github.com/unrolled/secure)</code> - Security Middleware    
<code>[Achieving a Perfect SSL Labs Score with Go](https://blog.bracebin.com/achieving-perfect-ssl-labs-score-with-go)</code>  
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>
<code>[]()</code>

---

## **STANDARD LIBRARY**

### **FMT**

`import "fmt"`

`fmt.Print()` - pritn  
`fmt.Println()` - print and new line  
`fmt.Printf()` - print with a certain format   

```go
type point struct {
    x, y int
}
p := point{1, 2}

fmt.Printf("%v\n", p)                   // {1 2}

// includes the names of the fields in the struct  
fmt.Printf("%+v\n", p)                  // {x:1 y:2}

// Print the type of a value  
fmt.Printf("%T\n", p)                   // main.point

// `%d` for standard integers
fmt.Printf("%d\n", 123)                 // 123

// Print the character that corresponds to the whole  
fmt.Printf("%c\n", 65)                  // a

// print floats
fmt.Printf("%f\n", 78.9)                // 78.90000

// print basic string `%s`.
fmt.Printf("%s\n", "\"string\"")        // "string"

// print Boolean
fmt.Printf("%t\n", a ==b)               // true o false

// print pointer `%p`.
fmt.Printf("%p\n", &p)                  // 0x42135100
```

`fmt.Sprint()` - returns the result to a string   
`fmt.Sprintln()` - returns the result with line break to a string      
`fmt.Sprintf()` - returns the result with a certain format to a string  

```go
s := fmt.Sprintf("Hi, my name is %s and I'm %d years old.", "Bob", 23)
// "Hi, my name is Bob and I'm 23 years old."
```

`fmt.Scan()` - to read a word from the keyboard, store successive values
separated by a space in successive arguments. Line breaks count as
space  
`fmt.Scanln()` - to read a word from the keyboard, store successive values
separated by a space in successive arguments. Line breaks end with the
reading of data  

* **verbs**

\- General  
`%v` - value in default format. In structs `%+v` add the names of the fields  
`%T` - data type    
`%#v` - representation of the type of the value with syntax of golang    
\- Boolean   
`%t` - boolean, returns true or false     
\- Integer   
`%b` - returns in base 2   
`%c` - returns character represented by the corresponding Unicode code  
`%d` - returns in base 10     
`%U` - Unicode format   
\- Floating point   
`f%` - decimal notation without exponents      
`e%` - decimal notation with exponents  
\- Strings and []byte  
`%s` - standard string   
`%q` - to escape double quotes    
`%x` - convert to hexadecimal basis    

---

### **STRINGS**

`import "strings"`  

`strings.Contains("test", "es")` = true - Contains "test" to "es"      
`strings.Count("test", "t")` = 2 - How many "t" are on "test"  
`strings.HasPrefix("test", "te")` = true - Does "test" begin with "te"    
`strings.HasSuffix("test", "st")` = True -  Does "test" finish with "st"    
`strings.Index("test", "e")` = 1 - Returns string "e" position. If not exists returns -1  
`strings.Join([]string{"a","b"}, "-")` = "a-b" - Take a list of strings and put them together in one separated by another string ("-" in the Example)   
`strings.Repeat("a", 5)` = aaaaa -  Repeat a string n times   
`strings.Replace("aaaa", "a", "b", 2)` = "bbaa" - replace part of a chain with another part n times (or as many as possible if we pass -1)  
`strings.Split("a-b-c-d-e", "-")` = []string{"a","b","c","d","e"} - Split a string into an array of strings using another string as a separator  
`strings.ToLower("test")` = "TEST "     
`strings.ToUpper("TEST")` = "test"     
`strings.Fields("i am a string")` = as split but using blank spaces. Equivalent to `strings.Split (text," ")`  
`strings.Trim("cadena","loquecorta")` = removes all loquecorta but only from the beginning and end  
strings.Trim(" !!! Achtung! Achtung! !!! ", "! ") == ["Achtung! Achtung"]  

Convert string to slice of bytes and vice versa

`arr := []byte("test")`  
`str := string([]byte{'t','e','s','t'})`

Outside the string package

`len("iamastring")` - return string length  
`"cadena"[3]` - returns ASCII code of the character of index 3, "e" = 101   
`string(cadena[n])` - returns the character of the chain in the n position  

---

### **STRCONV**

`import "strconv"` - conversions between numbers and strings  

`s := strconv.Itoa(-42)` - int to string  
`i, err := strconv.Atoi("-42")` - string to int  
`b, err := strconv.ParseBool("true")` - string to boolean  
`f, err := strconv.ParseFloat("3.1415", 64)` - string to float  
`i, err := strconv.ParseInt("-42", 10, 64)` - string to int  
`u, err := strconv.ParseUint("42", 10, 64)` - string to uint  
`s := strconv.FormatBool(true)` - boolean value to string  
`s := strconv.FormatFloat(3.1415, 'E', -1, 64)` - float to string  
`s := strconv.FormatInt(-42, 16)` - int to string  
`s := strconv.FormatUint(42, 16)` - uint to string  

---

### **APPEND**

[Slice tricks](https://github.com/golang/go/wiki/SliceTricks) 

[More](https://www.reddit.com/r/golang/comments/283vpk/help_with_slices_and_passbyreference/)

`func append(slice []T, elements...T) []T.`

---

### **IO**

`import "io"`    

It has two main interfaces

* **Reader**
supports reading through the Read method

* **Writer**
supports writing through the Write method

### **IO/IOUTIL**

`import io/ioutil`    

* **read and write a file**

More control through a `File struct` of the OS package

```go
data := []byte("Hello World!\n")

// write 
err := ioutil.WriteFile("data1", data, 0644)
if err != nil {
    panic(err)
}

//read
read, err := ioutil.ReadFile("data1")
if err != nil {
    return
}
fmt.Print(string(read1))
```

* **Limit io size**

```go
defer resp.Body.Close()
limitReader := &io.LimitedReader{R: resp.Body, N: 2e6} // (2mb)
body, err := ioutil.ReadAll(limitReader)
```

---

### **OS**

`import "os"`  

* **Get current dir**

```go
os.Getwd()
```

* **Read/Write Files**  

```go
// One way
file, err := os.Open("test.txt")
if err != nil {
    // handle the error here
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
```

```go
// another way
data := []byte("Hello World!\n")

// write to file and read from file using the File struct
file1, _ := os.Create("data2")
defer file1.Close()

bytes, _ := file1.Write(data)
fmt.Printf("Wrote %d bytes to file\n", bytes)

file2, _ := os.Open("data2")
defer file2.Close()

read2 := make([]byte, len(data))
bytes, _ = file2.Read(read2)
fmt.Printf("Read %d bytes from file\n", bytes)
fmt.Println(string(read2))
```

* **Create File**  

```go
func main() {
  file, err := os.Create("test.txt")
  if err != nil {
      return
  }
  defer file.Close()
  file.WriteString("test")
}
```

* **Read the contents of a directory**

`Readdir` - take an argument that is the number of entries it returns. With -1
returns all

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

`Walk` - to recursively go through a directory. Belongs to the package
[path/filepath](#pathfilepath)  

* **Command line arguments**

the first value of the slice of arguments is the name of the path command included  

`argsWithProg := os.Args`- full slice with command name path included  
`argsWithoutProg := os.Args[1:]` - slice only of arguments   
`arg := os.Args[x]` - returns arg with position X    

* **environment variables**  

`os.Setenv("varName", "value")` - sets a key / value pair for an environment variable  
`os.Getenv("varName")` - returns the value of that key   

```go
// os.Environ is a list of all environment variables  
for _, e := range os.Environ() {
    pair := strings.Split(e, "=")
		fmt.Println(pair[0], "-->", pair[1])
}
```

---

### **PATH/FILEPATH**

`import path/filepath`    

* **Recurring a directory**

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

### **REGEXP**

`import "regexp"`  

```go
// Check if it's a string  
patron := "loquequeremoscomprobar"
match, _ := regexp.MatchString("p([a-z]+)ch", patron)
fmt.Println(match)

// or compile a struct optimized for regexp first  
patron := "loquequeremoscomprobar"
r, _ := regexp.Compile("p([a-z]+)ch")
fmt.Println(r.MatchString(patron))
```

```go
// By Example change in the string s the underscores by dashes  
r := regexp.MustCompile("_")
s = r.ReplaceAllString(s, `-`)
```

---

### **JSON**

`import "encoding/json"`  

[Golang JSON](https://blog.golang.org/json-and-go)  

`dataJson, err := json.Marshal(structObject)` - Go struct data to JSON data    
`dataJson, err:= json.MarshalIndent(strObj, "", "  ")` - nice preformatted    

`err := json.Unmarshal(dataJson, &structObject)` - JSON data to Go struct data  

```go
urlDir := "https://jolav.me/path/to/some/api/"
resp, err := http.Get(urlDir)
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()

body, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
body = body[5 : len(body)-6] // remove <pre></pre>

var s structObject
err = json.Unmarshal(body, &s)
fmt.Println(s)
```

* **json to go struct**  

[JSON-to-Go online](https://mholt.github.io/json-to-go/)

`json:"-"` ignore that field either by converting to json or by converting from json  
`json:"fieldname,omitempy"` - the field is not included when converting to json if that field already has a default value.  

* **Decoder**

In addition to `Unmarshal/Marshal` there is `Decoder/Encoder`, which must be used if the data comes from a stream io.Reader as for example the Body of an http.Request.  
If the data is in a string or in memory, better use Unmarshal

```go
type configuration struct { something }

file, _ := os.Open("conf.json")
defer file.Close()
decoder := json.NewDecoder(file)
conf := configuration{}
err := decoder.Decode(&conf)
if err != nil {
    fmt.Println("Error:", err)
}
fmt.Println(conf)
```

* **Parsing Interfaces**

Parsing into an Interface  
If you have truly no idea what your JSON might look like, you can parse it into a generic interface{}. If you’re not familiar an empty interface{} is a way of defining a variable in Go as “this could be anything”. At runtime Go will then allocate the appropriate memory to fit whatever you decide to store in it.  
This is what it looks like:  

```go
var parsed interface{}
err := json.Unmarshal(data, &parsed)
``` 

Actually using parsed is a bit laborious, as Go can’t use it without knowing what type it is. You can end up with code which tries the kitchen sink:

```go
switch parsed.(type) {
    case int:
        someGreatIntFunction(parsed.(int))
    case map:
        someMapThing(parsed.(map))
    default:
        panic("JSON type not understood")
}
```

You can also do similar type assertions inline:

```go
intVal, ok := parsed.(int)
if !ok {
    panic("JSON value must be an int")
}
```

Fortunately however, it’s rare to truly have no idea what a value might be. If, for example, you know your JSON value is an object, you can parse it into a map[string]interface{}. This gives you the advantage of being able to refer to specific keys. An example:

```go
var parsed map[string]interface{}
data := []byte(`
    {
        "id": "k34rAT4",
        "age": 24
    }
`)
err := json.Unmarshal(data, &parsed)
```

You can then refer to specific keys without a problem:

```go
parsed["id"]
````

You still have interfaces as the value of your map however,  
so you must do type assertions to use them:

```go
idString := parsed["id"].(string)
```

Go uses these six types for all values parsed into interfaces:

```go
bool, for JSON booleans
float64, for JSON numbers
string, for JSON strings
[]interface{}, for JSON arrays
map[string]interface{}, for JSON objects
nil for JSON null
```

Meaning, your numbers will always be of typefloat64, and will need to be casted to int, for example. If you have a particular need to get ints directly, you can use the UseNumber method. Which gives you an object which can be converted to either a float64 or an int at your discretion.

Similarly, all objects decoded into an interface will be map[string]interface{}, and will need to be manually mapped to whatever struct you may wish to place them in.

[Sobre JSON](https://eager.io/blog/go-and-json/)

---

### **TIME**

`import "time"`  

`now := time.Now()` - returns current time 2012-10-31 15:50:13.793654 +0000 UTC

`then := time.Date(2015, 10, 10, 18, 30, 08, 0, time.UTC)` - We create a time struct associated with a location (time zone)  

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

`diff := now.Sub(then)` - Sub method returns duration of the interval between two times  

```go
diff.Hours()
diff.Minutes()
diff.Seconds()
diff.Nanoseconds()
// move forward or backward in time  
then.Add(diff)
then.Add(-diff)

// add Time
anyTime.Add( 1000 * time.Hours)
```

* **String to Time**


```go
// pass a date to string according to a certain format  
layout := "2006-01-02 15:04:05"  
t, err := time.Parse(layout1, start)
if err != nil {
    fmt.Prinltln(err)
}

// Current time to string with a certain format  
horaActual = time.Now().Format(layout)
```

* **Time to String**

```go
myString = myTime.String()
```

* **Timestamp**

`Unix epoch`

```go
now := time.Now()
secs := now.Unix()
nanos := now.UnixNano()
millis := nanos / 1000000

time.Unix(secs, 0)
time.Unix(0, nanos)
```

* **Intervals**

`timers` - to do something once in a while  

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
// Timer 2 expired NEVER APPEARS, it is canceled before
```

`tickers` - to do something repeatedly at regular intervals  

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

### **MATH**

`import "math"`  

`math.Floor(x float64) float64` - returns the largest integer (int) less than or equal to x  
`math.Pow(x,y float64) float64` - x to the power of y    

---

### **MATH/RAND**

`import "math/rand"`  

`rand.Intn(10)`- generates an integer random number >= 0 and <10  
`rand.Float64()`- generates random numer >= 0.0 y < 1.0  
`(rand.Float64()*5)+5`- genera entre >= 5.0 y < 10.0   

`s1 := rand.NewSource(time.Now().UnixNano())`- random seed  
`r1 := rand.New(s1)`- change seed    

`rand.Seed(time.Now().UTC().UnixNano())` - another way to change the seed so that it is not always the same  

```go
func init() {
	//fmt.Println(`Init from package tracker`)
	r = rand.New(rand.NewSource(time.Now().UnixNano()))
}
var r *rand.Rand
func createRandomString() string {
	const chars = "abcdefghijklmnopqrstuvwxyz0123456789"
	result := ""
	for i := 0; i < lenID; i++ {
		result += string(chars[r.Intn(len(chars))])
	}
	return result
}
```

---

### **DATABASE/SQL**

[database/sql](http://go-database-sql.org/index.html)

---

### **FLAG**

`import "flag"`  

To send arguments to a command  

[Examples](https://gobyexample.com/command-line-flags)  

---

### **SORT**

`import sort`    

* **s = []strings**

`sort.strings(s)` -  From lower to higher alphabetically  

* ** n = []ints || float32 || float64**

`sort.Ints(n)` - Sort the numbers from lowest to highest   
`sort.IntsAreSorted(n)` - Boolean that returns if they are ordered  

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



