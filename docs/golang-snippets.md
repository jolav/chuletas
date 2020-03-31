# GOLANG SNIPPETS

---

## SEND/RECEIVE DATA

### SendJSONToClient   

```go

import (
	"encoding/json"
	"log"
	"net/http"
)

// SendJSONToClient ...
func SendJSONToClient(w http.ResponseWriter, d interface{}) {
	w.Header().Set("Content-Type", "application/json")
	var dataJSON = []byte(`{}`)
	dataJSON, err := json.MarshalIndent(d, "", " ")
	if err != nil {
		log.Printf("ERROR Marshaling %s\n", err)
		w.Write([]byte(`{}`))
	}
	w.Write(dataJSON)
}
```

### SendXMLToClient   

```go

import (
	"encoding/xml"
	"log"
	"net/http"
)

// SendXMLToClient ...
func SendXMLToClient(w http.ResponseWriter, d interface{}) {
	w.Header().Set("Content-Type", "application/xml")
	var dataXML = []byte(`<output></output>`)
	dataXML, err := xml.Marshal(&d)
	if err != nil {
		log.Printf("ERROR Parsing into XML %s\n", err)
		w.Write([]byte(`{}`))
	}
	w.Write(dataXML)
}
```

### SendErrorToClient

```go
import (
	"encoding/json"
	"log"
	"net/http"
)

// SendErrorToClient ...
func SendErrorToClient(w http.ResponseWriter, d interface{}) {
	w.WriteHeader(http.StatusBadRequest)
	w.Header().Set("Content-Type", "application/json")
	var dataJSON = []byte(`{}`)
	dataJSON, err := json.MarshalIndent(d, "", " ")
	if err != nil {
		log.Printf("ERROR Marshaling %s\n", err)
		w.Write([]byte(`{}`))
	}
	w.Write(dataJSON)
}
```

### DoGetRequest

```go
import (
	"encoding/json"
	"io/ioutil"
	"log"
	"net/http"
	"time"
)

// DoGetRequest ...
func DoGetRequest(w http.ResponseWriter, url string, d interface{}) {
	var netClient = &http.Client{
		Timeout: time.Second * 10,
	}
	resp, err := netClient.Get(url)
	if err != nil {
		log.Fatal(err)
	}

	if resp.StatusCode != 200 {
		log.Fatal(err)
		return
	}

	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}
	// body is a string, for use we must Unmarshal over a struct
	err = json.Unmarshal(body, &d)
	if err != nil {
		log.Fatalln(err)
	}
}
```

### DoGetConcurrentRequest

```go
import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
)

func fillDefaultStocks(links []string) {
	ch := make(chan []byte)
	for _, link := range links {
		go doGetConcurrentRequest(link, ch)
	}
	for range links {
		json.Unmarshal(<-ch, &stocks)
	}
}

func doGetConcurrentRequest(url string, ch chan<- []byte) {
	resp, err := http.Get(url)
	if err != nil {
		msg := fmt.Sprintf("ERROR 1 HTTP Request %s", err)
		log.Printf(msg)
		ch <- []byte(msg)
		return
	}
	if resp.StatusCode != 200 {
		msg := fmt.Sprintf("ERROR 2 HTTP Request Status Code %d", resp.StatusCode)
		log.Printf(msg)
		ch <- []byte(msg)
		return
	}
	defer resp.Body.Close()
	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		msg := fmt.Sprintf("ERROR 3 HTTP Request %s", err)
		log.Printf(msg)
		ch <- []byte(msg)
		return
	}
	ch <- body
}
```

### DoPostRequest

```go
func sendRequest(data []byte, a app) {
	server, _ := os.Hostname()
	server = strings.ToLower(server)
	contentType := "application/x-www-form-urlencoded" //; param=value"
	host := "https://" + a.Conf.Host + a.Conf.PathToAPI

	var client = &http.Client{
		Timeout: time.Second * 3,
	}
	params := url.Values{
		//"server": {server},
		"test": {a.Conf.Valid},
		"data": {string(data)},
	}
	params.Add("server", server)

	query := bytes.NewBufferString(params.Encode())
	resp, err := http.NewRequest("POST", host, query)
	resp.Header.Add("Content-Type", contentType)
	resp.Header.Add("Accept-Charset", "utf-8")
	if err != nil {
		log.Fatal("Error preparing POST request => ", err)
	}

	r, err := client.Do(resp)
	if err != nil {
		log.Fatal("Error sending POST request => ", err)
	}
	if r.StatusCode != 200 {
		log.Fatal("Error received from server => ", r.Status, " ", err)
	}
}
```

### GetInterfacesTypes 

```go
import "fmt"

// GetInterfacesTypes ...
func GetInterfacesTypes(f interface{}) {
	switch vf := f.(type) {
	case map[string]interface{}:
		//fmt.Println("is a map:")
		for k, v := range vf {
			switch vv := v.(type) {
			case string:
				//fmt.Printf("%v: is string - %q\n", k, vv)
			case int:
				//fmt.Printf("%v: is int - %q\n", k, vv)
			case float64:
				//fmt.Printf("%v: is float64 - %g\n", k, vv)
			default:
				fmt.Sprintln(k, v, vv)
				//fmt.Printf("%v: ", k)
				GetInterfacesTypes(v)
			}
		}
	case []interface{}:
		//fmt.Println("is an array:")
		for k, v := range vf {
			switch vv := v.(type) {
			case string:
				//fmt.Printf("%v: is string - %q\n", k, vv)
			case int:
				//fmt.Printf("%v: is int - %q\n", k, vv)
			case float64:
				//fmt.Printf("%v: is float64 - %g\n", k, vv)
				if k == 4 {
					fmt.Println(`ALELUYA==>`, vv)
				}
			default:
				fmt.Sprintln(k, v, vv)
				//fmt.Printf("%v: ", k)
				GetInterfacesTypes(v)
			}
		}
	}
}
```

### IsJSON

```go
import "encoding/json"
// IsJSON ...

func IsJSON(str string) bool {
	var js json.RawMessage
	return json.Unmarshal([]byte(str), &js) == nil
}
```

---

## ERRORCHECK

### Check

```go
import "log"

// Check ...
func Check(err error) {
	if err != nil {
		log.Fatal(err)
	}
}
```

---

## FILES

### ReadFile 

```go
import (
	"log"
	"os"
)

// ReadFile ...
func ReadFile(filePath string) string {
	file, err := os.Open(filePath)
	defer file.Close()
	stat, err := file.Stat()
	if err != nil {
		log.Fatal(err)
	}
	bs := make([]byte, stat.Size())
	_, err = file.Read(bs)
	if err != nil {
		log.Fatal(err)
	}
	data := string(bs)
	return data
}
```

### ReadFileLineByLine  

```go
import (
	"bufio"
	"fmt"
	"log"
	"os"
)

// ReadFileLineByLine ...
func ReadFileLineByLine(filePath string) {
	file, err := os.Open(filePath)
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()
	scanner := bufio.NewScanner(file)
	for scanner.Scan() {
		line := scanner.Text()
		fmt.Println(line)
	}
	if err := scanner.Err(); err != nil {
		log.Fatal(err)
	}
}
```

### WriteFile  

```go
import (
	"log"
	"os"
)

// WriteFile ...
func WriteFile(filePath string, content string) {
	file, err := os.Create(filePath)
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()
	file.WriteString(content)
}

```

### LoadJSONfromFileDecoder   

```go
import (
	"encoding/json"
	"log"
	"os"
)

// LoadJSONfromFileDecoder ...
func LoadJSONfromFileDecoder(filePath string, data interface{}) {
	file, err := os.Open(filePath)
	if err != nil {
		log.Fatalln("Cannot open config file", err)
	}
	defer file.Close()
	decoder := json.NewDecoder(file)
	err = decoder.Decode(&data)
	if err != nil {
		log.Fatalln("Cannot get configuration from file", err)
	}
}
```

### LoadJSONfromFileMarshall  

```go
import (
	"encoding/json"
	"io/ioutil"
	"log"
	"os"
)

// LoadJSONfromFileMarshall ...
func LoadJSONfromFileMarshall(filePath string, data interface{}) {
	file, err := os.Open(filePath)
	if err != nil {
		log.Fatalln("Cannot open config file", err)
	}
	defer file.Close()
	body, err := ioutil.ReadAll(file) //	get file content
	if err != nil {
		log.Fatalln(err)
	}
	err = json.Unmarshal(body, &data)
	if err != nil {
		log.Fatalln(err)
	}
}
```

### WriteJSONtoFile  

```go
import (
	"encoding/json"
	"os"
)

// WriteJSONtoFile ...
func WriteJSONtoFile(filePath string, d interface{}) {
	f, err := os.Create(filePath)
	if err != nil {
		panic(err)
	}
	defer f.Close()
	e := json.NewEncoder(f)
	e.Encode(&d)
}
```

### DownloadFile  

```go
import (
	"io"
	"net/http"
	"os"
)

// DownloadFile ...
func DownloadFile(filePath string, url string) (err error) {
	// Create the file
	out, err := os.Create(filePath)
	if err != nil {
		return err
	}
	defer out.Close()
	// Get the data
	resp, err := http.Get(url)
	if err != nil {
		return err
	}
	defer resp.Body.Close()
	// Writer the body to file
	_, err = io.Copy(out, resp.Body)
	if err != nil {
		return err
	}
	return nil
}
```

### SendFileFromServerToClient

```go
import (
	"fmt"
	"io"
	"net"
	"net/http"
	"time"
)

func Index(w http.ResponseWriter, r *http.Request) {
	url := "http://upload.wikimedia.org/wikipedia/en/b/bc/Wiki.png"

	timeout := time.Duration(5) * time.Second
	transport := &http.Transport{
		ResponseHeaderTimeout: timeout,
		Dial: func(network, addr string) (net.Conn, error) {
			return net.DialTimeout(network, addr, timeout)
		},
		DisableKeepAlives: true,
	}
	client := &http.Client{
		Transport: transport,
	}
	resp, err := client.Get(url)
	if err != nil {
		fmt.Println(err)
	}
	defer resp.Body.Close()

	//copy the relevant headers. If you want to preserve the downloaded 
	// file name, extract it with go's url parser.
	w.Header().Set("Content-Disposition", "attachment; filename=Wiki.png")
	w.Header().Set("Content-Type", r.Header.Get("Content-Type"))
	w.Header().Set("Content-Length", r.Header.Get("Content-Length"))

	//stream the body to the client without fully loading it into memory
	io.Copy(w, resp.Body)
}

func main() {
	http.HandleFunc("/", Index)
	err := http.ListenAndServe(":8000", nil)

	if err != nil {
		fmt.Println(err)
	}
}
```

### ParseCSVFile


```csv
// file.csv
"AAPL","Apple Inc","4.10%"
"AMZN","Amazon.com Inc","3.49%"
"MSFT","Microsoft Corp","3.23%"
"GOOGL","Alphabet Inc","3.09%"
```

```go
import (
	"bufio"
	"encoding/csv"
	"encoding/json"
	"fmt"
	"io"
	"os"
)

type stock struct {
	Symbol string `json:"symbol"`
}

func main() {
	csvFile, _ := os.Open("sp.csv")
	reader := csv.NewReader(bufio.NewReader(csvFile))
	var stocks []stock
	for {
		line, error := reader.Read()
		if error == io.EOF {
			break
		}
		aux := stock{
			Symbol: line[0],
		}
		stocks = append(stocks, aux)
	}
	//stocksJSON, _ := json.Marshal(stocks)
	//fmt.Println(string(stocksJSON))

	f, err := os.Create("spList.json")
	if err != nil {
		panic(err)
	}
	defer f.Close()
	e := json.NewEncoder(f)
	e.Encode(&stocks)
	fmt.Println(`END`)
}
```

```json
// result
[
  {
    "symbol": "AAPL"
  },
  {
    "symbol": "AMZN"
  },
  {
    "symbol": "MSFT"
  },
  {
    "symbol": "GOOGL"
	}
]
```

---

## HTTP SERVER

Lectura 

[Request Handling in Go](https://www.alexedwards.net/blog/a-recap-of-request-handling)


```go
type Handler interface {
	ServeHttp( ResponseWriter, *Request )
}
```

### Wrapper 

Es muy sencillo pero luego complica para testearlo

```go
mux.HandleFunc("/path", func(w http.ResponseWriter, r *http.Request) {
		nombreFuncion(w, r, loQueQueramosPasar)
})
```

### Handle + HandleFunc

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

func timeHandler1(w http.ResponseWriter, r *http.Request) {
	tm := time.Now().Format(time.RFC1123)
	fmt.Println("/time/" + tm)
	w.Write([]byte("The time is: " + tm))
}

func timeHandler2(format string) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request){
		tm := time.Now().Format(format)
		fmt.Println("/time/" + tm)
		w.Write([]byte("The time is: " + tm))
	})
}

/*	lo mismo pero con conversion implicita al tipo HandlerFunc
func timeHandler2(format string) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		tm := time.Now().Format(format)
		fmt.Println("/time/" + tm)
		w.Write([]byte("The time is: " + tm))
	}
}
*/

func helloHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("/hello")
	w.Write([]byte("/hello"))
}

func main() {
	mux := http.NewServeMux()

	// Creamos un closure con las variables que queremos usar
	th2 := timeHandler2(time.RFC3339)

	mux.HandleFunc("/time/1", timeHandler1)
	mux.Handle("/time/2", th2)
	mux.HandleFunc("/hello", helloHandler)

	//http.HandleFunc("/time/1", timeHandler1)
	//http.Handle("/time/2", th2)
	//http.HandleFunc("/hello", helloHandler)

	http.ListenAndServe(":3000", mux /*nil*/)
}

```

### Handler

```go
type specificHandler struct {
	Thing string
}

func(h *specificHandler)ServeHTTP(w http.ResponseWriter,r *http.Request) {
	w.Write(h.Thing)
}

func main() {
  http.Handle("/something", &specificHandler{Thing: "Hello world!"})
  http.ListenAndServe(":8080", nil)
}
```

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

type timeHandler struct {
	format string
}

func (th *timeHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	tm := time.Now().Format(th.format)
	fmt.Println("/time/" + tm)
	w.Write([]byte("The time is: " + tm))
}

type helloHandler struct{}

func (ti *helloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	fmt.Println("/hello")
	w.Write([]byte("/hello"))
}

func main() {
	/mux := http.NewServeMux()

	th1 := &timeHandler{format: time.RFC1123}
	th2 := &timeHandler{format: time.RFC3339}
	hi := &helloHandler{}
	
	mux.Handle("/time/1", th1)
	mux.Handle("/time/2", th2)
	mux.Handle("/hello", hi)

	//http.Handle("/time/1", th1)
	//http.Handle("/time/2", th2)
	//http.Handle("/hello", hi)

	http.ListenAndServe(":3000", /*nil*/ mux)
}
```



---

## MIDDLEWARES

### RateLimit  

```go
import "net/http"

// ExceedLimit ...
func ExceedLimit(ip string) bool {
	// ToDO
	return false
}

// RateLimit ...
func RateLimit(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		if ExceedLimit(GetIP(r)) {
			http.Error(w, "Too many requests", http.StatusTooManyRequests)
			return
		}
		next.ServeHTTP(w, r)
	})
}
```

---

## NETWORK

### GetIP  

```go
import (
	"net"
	"net/http"
)

// GetIP returns string with IP
func GetIP(r *http.Request) string {
	ip := r.Header.Get("X-Forwarded-For")
	if len(ip) > 0 {
		return ip
	}
	ip, _, _ = net.SplitHostPort(r.RemoteAddr)
	return ip
}
```

### IsValidURL  

```go
import "net/url"

// IsValidURL ...
func IsValidURL(rawurl string) bool {
	_, err := url.ParseRequestURI(rawurl)
	if err != nil {
		return false
	}
	return true
}
```

### GetLanguage  

```go
import (
	"net/http"
	"strings"
)

// GetLanguage ...
func GetLanguage(r *http.Request) string {
	lang := r.Header.Get("Accept-Language")
	if lang != "" { // curl request doesnt have Accept-Language
		lang = lang[0:strings.Index(lang, ",")]
	}
	return lang
}
```

### RemoveProtocolFromURL

```go
// RemoveProtocolFromURL ...
func RemoveProtocolFromURL(url string) string {
	if strings.HasPrefix(url, "https://") {
		return url[8:]
	}
	if strings.HasPrefix(url, "https:/") {
		return url[7:]
	}
	if strings.HasPrefix(url, "http://") {
		return url[7:]
	}
	if strings.HasPrefix(url, "http:/") {
		return url[6:]
	}
	return url
}
```

### RemoveProtocolAndWWWFromURL

```go
// RemoveProtocolAndWWWFromURL ...
func RemoveProtocolAndWWWFromURL(url string) string {
	if strings.HasPrefix(url, "https://www.") {
		return url[12:]
	}
	if strings.HasPrefix(url, "https:/www.") {
		return url[11:]
	}
	if strings.HasPrefix(url, "http://www.") {
		return url[11:]
	}
	if strings.HasPrefix(url, "http:/www.") {
		return url[10:]
	}
	return RemoveProtocolFromURL(url)
}
```

---

## NUMBERS

### GetRandomInt  

```go
import (
	"math/rand"
	"time"
)

// GetRandomInt [min, max] both included
func GetRandomInt(min, max int) int {
	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	//rand.Seed(time.Now().UnixNano())
	//	return rand.Intn(max-min+1) + min
	return r.Intn(max-min+1) + min
}
```

### GetRandomFloat64  

```go
import (
	"math/rand"
	"time"
)

// GetRandomFloat64 [min, max] both included
func GetRandomFloat64(min, max float64) float64 {
	rand.Seed(time.Now().UnixNano())
	return (rand.Float64() * (max - min)) + (min)
}
```

### ToFixedFloat64  

```go
import "math"

// ToFixedFloat64 (untruncated, num) -> untruncated.toFixed(num)
func ToFixedFloat64(untruncated float64, precision int) float64 {
	coef := math.Pow10(precision)
	truncated := float64(int(untruncated*coef)) / coef
	return truncated
}
```

### ToFixedFloat32  

```go
import "math"

// ToFixedFloat32 (untruncated, num) -> untruncated.toFixed(num)
func ToFixedFloat32(untruncated float32, precision int) float32 {
	coef := float32(math.Pow10(precision))
	truncated := float32(int(untruncated*coef)) / coef
	return truncated
}
```

### RoundFloat64  

```go
// RoundFloat64 -> rounds float64 into integer
func RoundFloat64(num float64) int {
	if num < 0 {
		return int(num - 0.5)
	}
	return int(num + 0.5)
}
```

### RoundFloat32  

```go
// RoundFloat32 -> rounds float32 into integer
func RoundFloat32(num float32) int {
	if num < 0 {
		return int(num - 0.5)
	}
	return int(num + 0.5)
}
```

### ReverseSliceInt  

```go
// ReverseSliceInt [0,1,2,3,4,5] ==> [5,4,3,2,1,0]
func ReverseSliceInt(reverse []int) []int {
	for i, j := 0, len(reverse)-1; i < j; i, j = i+1, j-1 {
		reverse[i], reverse[j] = reverse[j], reverse[i]
	}
	return reverse
}
```

### TransposeMatrixInt  

```go
// TransposeMatrixInt rows > cols or cols > rows
// but rows.elements >= cols.elements
func TransposeMatrixInt(matrix [][]int) [][]int {
	result := make([][]int, len(matrix[0]))
	for i := range result {
		result[i] = make([]int, len(matrix))
	}
	for y, v := range matrix {
		for x, t := range v {
			result[x][y] = t
		}
	}
	return result
}
```

### SliceContainsInt  

```go
// SliceContainsInt ... returns true/false
func SliceContainsInt(num int, slice []int) bool {
	for _, v := range slice {
		if v == num {
			return true
		}
	}
	return false
}
```

### ArabicToRomanNumbers  

```go
import "math"

// ArabicToRomanNumbers converts arabic int to roman numeral (string)
func ArabicToRomanNumbers(n int) string {
	var rom string
	hundreds := []string{"C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"}
	tens := []string{"X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"}
	units := []string{"I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"}
	t := int(math.Floor(float64(n / 1000)))
	h := int(math.Floor(float64(n % 1000 / 100)))
	d := int(math.Floor(float64(n % 100 / 10)))
	u := int(math.Floor(float64(n % 10)))
	for i := 0; i < t; i++ {
		rom += "M"
	}
	if h > 0 {
		rom += hundreds[h-1]
	}
	if d > 0 {
		rom += tens[d-1]
	}
	if u > 0 {
		rom += units[u-1]
	}
	return rom
}

```

---

## STRINGS

### RemoveAllWhitespaces 

```go
import "strings"

// RemoveAllWhitespaces ...
func RemoveAllWhitespaces(str string) string {
	return strings.Replace(str, " ", "", -1)
}
```

### ReplaceAllWhitespacesByChar  

```go
import "strings"

// ReplaceAllWhitespacesByChar ...
func ReplaceAllWhitespacesByChar(str string, otherChar string) string {
	return strings.Replace(str, " ", otherChar, -1)
}

```

### ReverseSliceString  

```go
// ReverseSliceString ["H","O","L","A"] ==> ["A","L","O","H"]
func ReverseSliceString(reverse []string) []string {
	for i, j := 0, len(reverse)-1; i < j; i, j = i+1, j-1 {
		reverse[i], reverse[j] = reverse[j], reverse[i]
	}
	return reverse
}
```

### TransposeMatrixString  

```go
// TransposeMatrixString rows > cols or cols > rows
// but rows.elements >= cols.elements
func TransposeMatrixString(matrix [][]string) [][]string {
	result := make([][]string, len(matrix[0]))
	for i := range result {
		result[i] = make([]string, len(matrix))
	}
	for y, v := range matrix {
		for x, t := range v {
			result[x][y] = t
		}
	}
	return result
}
```

### SliceContainsString  

```go
// SliceContainsString ... returns true/false
func SliceContainsString(str string, slice []string) bool {
	for _, v := range slice {
		if v == str {
			return true
		}
	}
	return false
}
```

### FixBadEncodedStrings

```go
// FixBadEncodedStrings ...
func FixBadEncodedStrings(bad string) string {
	//bad := "KlÃ¤der"
	var good []byte
	for _, c := range bad {
		good = append(good, byte(c))
	}
	//fmt.Println(string(good))
	return string(good)
}
```

---

## OS

### GenericCommand  

```go
import (
	"fmt"
	"os/exec"
)

func main()  {
	command := []string{"vnstat", "-i", ifinterface, "--json"}
	///fmt.Println("Command =>", command)
	chunk, err := genericCommand(command)
	if err != nil {
		log.Fatal(err)
	}
	//fmt.Println(`CHUNK =>`, string(chunk))
}

// GenericCommand ...
func GenericCommand(args []string) (err error) {
	_, err = exec.Command(args[0], args[1:len(args)]...).CombinedOutput()
	if err != nil {
		fmt.Println(err)
		return err
	}
	return err
}

// GenericCommand ...
func genericCommand(args []string) (chunk []byte, err error) {
	chunk, err = exec.Command(args[0], args[1:len(args)]...).CombinedOutput()
	if err != nil {
		return nil, err
	}
	return chunk, err
}

// GenericCommand ...
func GenericCommand(comm string) {
	_, err := exec.Command("sh", "-c", comm).CombinedOutput()
	if err != nil {
		log.Fatal(err)
	}
}
```

---

## TIME

### ParseStringToTime  

```go
import "time"

// ParseStringToTime ...
func ParseStringToTime(start string) time.Time {
	layout1 := "2006-01-02" // Layout numbers?
	layout2 := "2006-01-02T15:04:05"
	t, err := time.Parse(layout1, start)
	if err != nil {
		t, err = time.Parse(layout2, start)
	}
	return t
}
```

### GetTimestampFromDate  

```go
import (
	"strconv"
	"strings"
	"time"
)

// GetTimestampFromDateString ...(yyyy-mm-dd)
func GetTimestampFromDateString(date string) float64 {
	var t int64
	params := strings.Split(date, "-")
	day, _ := strconv.Atoi(params[2])
	month, _ := strconv.Atoi(params[1])
	year, _ := strconv.Atoi(params[0])
	auxT := time.Date(year, time.Month(month), day, 0, 0, 0, 0, time.UTC)
	t = int64(auxT.UnixNano() / int64(time.Millisecond))
	return float64(t)
}
```

### OnceADayTask

```go
import "time"

func onceADayTask() {
	t := time.Now()
	n := time.Date(t.Year(),t.Month(),t.Day(),3,10,10,0,t.Location())
	d := n.Sub(t)
	if d < 0 {
		n = n.Add(24 * time.Hour)
		d = n.Sub(t)
	}
	for {
		time.Sleep(d)
		d = 24 * time.Hour
		doSomeTask()
	}
}
```

### SetInterval

```go

import (
	"fmt"
	"net/http"
	"time"
)

type updates []struct {
	Symbol string  `json:"symbol"`
	Price  float64 `json:"price"`
}

func initUpdateIntervals() {
	var u updates
	var w http.ResponseWriter
	ticker := time.NewTicker(time.Millisecond * 1000)
	go func() {
		for _ = range ticker.C {
			u = updates{}
			lib.MakeGetRequest(w, url, &u)
			for _, v := range u {
				if v.Symbol != "" {
					stocks[v.Symbol].PriceNow = v.Price
				}
			}
			t := time.Now()
			hour, min, sec := t.Clock()
			fmt.Println(`TICK`, hour, min, sec)
			if hour == 6 && min == 1 {
				if sec > 0 && sec < 6 {
					go dailyWork()
				}
			}
		}
	}()
}
```

---

## LOGS

```go
// Esto para que todo lso que sea log.algo vaya al fichero elegido
if a.Config.Mode == "production" {
	var f = "log/errors.log"
	mylog, err := os.OpenFile(f, os.O_WRONLY|os.O_CREATE|os.O_APPEND, 0644)
	if err != nil {
		log.Fatal("ERROR opening log file %s\n", err)
	}
	defer mylog.Close() // defer must be in main
	log.SetOutput(mylog)
}
```


```golang
var f1 = "log/hits.log"
hitsLog, err := os.OpenFile(f1, os.O_WRONLY|os.O_CREATE|os.O_APPEND, 0644)
if err != nil {
	log.Fatal("ERROR opening log file %s\n", err)
}
var hitsLogger *log.Logger
hitsLogger = log.New(hitsLog, "Hits Logger:\t", log.Ldate|log.Ltime) 
hitsLogger.Print("Hola hitsLogger")
```

---

