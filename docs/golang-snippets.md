# GOLANG SNIPPETS

---

## SEND/RECEIVE DATA

### SendResponse   

```go
// SendResponse...
func SendResponse(w http.ResponseWriter, d interface{}, s int) {
	w.WriteHeader(s)
	if d == nil {
		return
	}
	w.Header().Set("Content-Type", "application/json")
	dataJSON, err := json.MarshalIndent(d, "", " ")
	if err != nil {
		log.Printf("ERROR Marshaling %v\n", err)
		http.Error(w, "Internal Server Error", http.StatusInternalServerError)
		return
	}
	_, err = w.Write(dataJSON)
	if err != nil {
		log.Printf("ERROR writing JSON response: %v\n", err)
	}
}

// SendtResponseXML ...
func SendResponseXML(w http.ResponseWriter, d interface{}, s int) {
	w.WriteHeader(s)
	if d == nil {
		return
	}
	w.Header().Set("Content-Type", "application/xml")
	dataXML, err := xml.MarshalIndent(d, "", "  ")
	if err != nil {
		log.Printf("ERROR Marshaling XML: %v\n", err)
		http.Error(w, "Internal Server Error", http.StatusInternalServerError)
		return
	}
	_, err = w.Write(dataXML)
	if err != nil {
		log.Printf("ERROR writing XML response: %v\n", err)
	}
}
```

### Fetch GET

```go
// FetchGET ...
func FetchGET(url string, d interface{}) error {
	client := &http.Client{
		Timeout: 10 * time.Second,
	}
	// with headers
	req, err := http.NewRequest("GET", url, nil)
	if err != nil {
		log.Printf("ERROR: creating request %s => %v", url, err)
		return err
	}
	req.Header.Set("Authorization", "Bearer ACCESS_TOKEN")
	req.Header.Set("Accept", "application/json")
	resp, err := client.Do(req) 
	if err != nil {
		log.Printf("ERROR: Request %s => %v", url, err)
		return err
	}

	// without Headers
	resp, err := client.Get(url)
	if err != nil {
		log.Printf("ERROR: Request %s => %v", url, err)
		return err
	}
	// common
	defer resp.Body.Close()

	if resp.StatusCode < 200 || resp.StatusCode >= 300 {
		err := errors.New("HTTP status:" + http.StatusText(resp.StatusCode))
		log.Printf("ERROR: %v", err)
		return err
	}

	decoder := json.NewDecoder(resp.Body)
	err = decoder.Decode(&d)
	if err != nil {
		log.Printf("ERROR unnmarshalling => %v", err)
		return err
	}

	return nil
}
```

### DoGetConcurrentRequest

```go
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
		msg := fmt.Sprintf("ERROR 2 Status Code %d", resp.StatusCode)
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

### fetchPOST

```go
// FetchPOST ...
func FetchPOST(url string, d interface{}) error {
	client := &http.Client{
		Timeout: 10 * time.Second,
	}

	jsonData, err := json.Marshal(d)
	if err != nil {
		log.Printf("ERROR: Failed to marshal request body => %v", err)
		return err
	}

	req, err := http.NewRequest("POST", url, bytes.NewBuffer(jsonData))
	if err != nil {
		log.Printf("ERROR: Failed to create request for %s => %v", url, err)
		return err
	}

	req.Header.Set("Authorization", "Bearer ACCESS_TOKEN")
	req.Header.Set("Accept", "application/json")
	req.Header.Set("Content-Type", "application/json")

	resp, err := client.Do(req)
	if err != nil {
		log.Printf("ERROR: Request %s => %v", url, err)
		return err
	}
	defer resp.Body.Close()

	if resp.StatusCode < 200 || resp.StatusCode >= 300 {
		err := errors.New("HTTP status: " + http.StatusText(resp.StatusCode))
		log.Printf("ERROR: %v", err)
		return err
	}

	decoder := json.NewDecoder(resp.Body)
	err = decoder.Decode(&d)
	if err != nil {
		log.Printf("ERROR unmarshalling => %v", err)
		return err
	}

	return nil
}

```

### fetchPOST with params

```go
import (
	url "net/url"
)

// FetchPOSTparams ...
func FetchPOSTparams(path string, d map[string]string) error {
	client := &http.Client{
		Timeout: 10 * time.Second,
	}

	params := url.Values{}
	for key, value := range d {
		params.Add(key, value)
	}
	/*params := url.Values{
		"test": {myTest},
		"data": {myData},
	}*/

	body := bytes.NewBufferString(params.Encode())

	req, err := http.NewRequest("POST", path, body)
	if err != nil {
		log.Printf("ERROR: Failed to create request for %s => %v", path, err)
		return err
	}

	req.Header.Set("Authorization", "Bearer YOUR_ACCESS_TOKEN")
	req.Header.Set("Accept-Charset", "utf-8")
	req.Header.Set("Content-Type", "application/x-www-form-pathencoded")

	resp, err := client.Do(req)
	if err != nil {
		log.Printf("ERROR: Request %s => %v", path, err)
		return err
	}
	defer resp.Body.Close()

	if resp.StatusCode < 200 || resp.StatusCode >= 300 {
		err := errors.New("HTTP status: " + http.StatusText(resp.StatusCode))
		log.Printf("ERROR: %v", err)
		return err
	}

	return nil
}
```

### GetInterfacesTypes 

```go
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
// IsJSON ...
func IsJSON(str string) bool {
	var js json.RawMessage
	return json.Unmarshal([]byte(str), &js) == nil
}
```

### IsPointer

```go
func IsPointer(d interface{}) bool {
	return reflect.TypeOf(d).Kind() == reflect.Ptr
}
```

---

## FILES

### ReadFile 

```go
// ReadFile ...
func ReadFile(filePath string) (string, error) {
	data, err := ioutil.ReadFile(filePath)
	if err != nil {
		return "", err
	}
	return string(data), nil
}

// ReadFileLineByLine ...
func ReadFileLineByLine(filePath string) ([]string, error) {
	file, err := os.Open(filePath)
	if err != nil {
		return nil, err
	}
	defer file.Close()

	var lines []string
	scanner := bufio.NewScanner(file)
	for scanner.Scan() {
		lines = append(lines, scanner.Text())
	}

	if err := scanner.Err(); err != nil {
		return nil, err
	}

	return lines, nil
}

```

### WriteFile  

```go
// WriteFile ...
func WriteFile(filePath string, content string) error {
	file, err := os.Create(filePath)
	if err != nil {
		return err
	}
	defer file.Close()

	_, err = file.WriteString(content)
	if err != nil {
		return err
	}

	return nil
}
```

### LoadJSON from File   

```go
// LoadJSONFIleDecoder ... use streams
func LoadJSONFileDecoder(filePath string, data interface{}) error {
	file, err := os.Open(filePath)
	if err != nil {
		return err
	}
	defer file.Close()

	decoder := json.NewDecoder(file)
	err = decoder.Decode(data)
	if err != nil {
		return err
	}

	return nil
}

// LoadJSONFileMarschall ... loads all
func LoadJSONFileMarshall(filePath string, data interface{}) error {
	file, err := os.Open(filePath)
	if err != nil {
		return err
	}
	defer file.Close()

	body, err := ioutil.ReadAll(file)
	if err != nil {
		return err
	}

	err = json.Unmarshal(body, data)
	if err != nil {
		return err
	}

	return nil
}

```

### WriteJSONtoFile  

```go
// WriteJSONtoFile ...
func WriteJSONtoFile(filePath string, d interface{}) error {
	f, err := os.Create(filePath)
	if err != nil {
		return err
	}
	defer f.Close()

	encoder := json.NewEncoder(f)
	err = encoder.Encode(d)
	if err != nil {
		return err
	}

	return nil
}
```

### DownloadFile  

```go
// DownloadFile ...
func DownloadFile(filePath string, url string) error {
	out, err := os.Create(filePath)
	if err != nil {
		return err
	}
	defer out.Close()

	resp, err := http.Get(url)
	if err != nil {
		return err
	}
	defer resp.Body.Close()

	if resp.StatusCode != http.StatusOK {
		return fmt.Errorf("ERROR Downloading file: %d", resp.StatusCode)
	}

	_, err = io.Copy(out, resp.Body)
	if err != nil {
		return err
	}

	return nil
}

```

### SendFileFromServerToClient

```go
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

func hiHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("/hello")
	w.Write([]byte("/hello"))
}

func main() {
	mux := http.NewServeMux()

	// Creamos un closure con las variables que queremos usar
	th2 := timeHandler2(time.RFC3339)

	mux.HandleFunc("/time/1", timeHandler1)
	mux.Handle("/time/2", th2)
	mux.HandleFunc("/hello", hiHandler)

	//http.HandleFunc("/time/1", timeHandler1)
	//http.Handle("/time/2", th2)
	//http.HandleFunc("/hello", hiHandler)

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

type hiHandler struct{}

func (ti *hiHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	fmt.Println("/hello")
	w.Write([]byte("/hello"))
}

func main() {
	/mux := http.NewServeMux()

	th1 := &timeHandler{format: time.RFC1123}
	th2 := &timeHandler{format: time.RFC3339}
	hi := &hiHandler{}
	
	mux.Handle("/time/1", th1)
	mux.Handle("/time/2", th2)
	mux.Handle("/hello", hi)

	//http.Handle("/time/1", th1)
	//http.Handle("/time/2", th2)
	//http.Handle("/hello", hi)

	http.ListenAndServe(":3000", /*nil*/ mux)
}
```

### Ejemplo Completo

```go
package main

import (
	"errors"
	"fmt"
	"log"
	"net/http"
	"os"
	"time"

	_ "github.com/go-sql-driver/mysql"
)

type app struct {
	Config struct {
		Mode          string `json:"mode"`
		Port          int    `json:"port"`
		Valid         string `json:"valid"`
		ErrorsLogFile string `json:"errorsLogFile"`
		HitsLogFile   string `json:"hitsLogFile"`
	} `json:"config"`
	Mysql struct {
		User      string `json:"user"`
		Password  string `json:"password"`
		DB        string `json:"db"`
		Host      string `json:"host"`
		Port      int    `json:"port"`
		TableBW   string `json:"tableBw"`
		TableHits string `json:"tableHits"`
	}
}

type requestError struct {
	Error      error  `json:"-"`
	Message    string `json:"message"`
	StatusCode int    `json:"-"`
}

func (a *app) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	r.ParseForm()
	valid := r.Form.Get("test")
	if valid != a.Config.Valid {
		http.Error(w, "Unauthorized", http.StatusUnauthorized)
		return
	}
	updateBw(w, r, a)
}

func main() {
	var a app
	loadConfigJSON(&a)
	checkMode(&a)

	// Custom Log File
	if a.Config.Mode == "production" {
		var f = a.Config.ErrorsLogFile
		mylog, err := os.OpenFile(f, os.O_WRONLY|os.O_CREATE|os.O_APPEND
				, 0644)
		if err != nil {
			log.Printf("ERROR opening log file %s\n", err)
		}
		defer mylog.Close() // defer must be in main
		log.SetOutput(mylog)
	}

	mux := http.NewServeMux()

	mux.Handle("/savebw/", &a)
	mux.Handle("/saveHits/", checkValid(
		func(w http.ResponseWriter, r *http.Request) {
			updateHits(w, r, &a)
		}, a.Config.Valid))
	mux.HandleFunc("/get/", checkValid(
		func(w http.ResponseWriter, r *http.Request) {
			getStats(w, r, &a)
		}, a.Config.Valid))
	mux.HandleFunc("/", badRequest)

	server := http.Server{
		Addr:           fmt.Sprintf("localhost:%d", a.Config.Port),
		Handler:        mux,
		ReadTimeout:    10 * time.Second,
		WriteTimeout:   30 * time.Second,
		MaxHeaderBytes: 1 << 20,
	}

	log.Printf("Server up listening %s in mode %s", server.Addr
			, a.Config.Mode)
	server.ListenAndServe()

}

func checkValid(next http.HandlerFunc, test string) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		r.ParseForm()
		valid := r.Form.Get("test")
		if valid != test {
			http.Error(w, "Unauthorized", http.StatusUnauthorized)
			return
		}
		next.ServeHTTP(w, r)
	}
}

func badRequest(w http.ResponseWriter, r *http.Request) {
	re := &requestError{
		Error:      errors.New("Unexistent Endpoint"),
		Message:    "Bad Request",
		StatusCode: 400,
	}
	sendErrorToClient(w, re)
}
```

---

## NETWORK

### GetIP  

```go
// GetIP ...
func GetIP(r *http.Request) string {
	headers := []string{
		"X-Forwarded-For",
		"X-Real-IP",
		"CF-Connecting-IP",
	}
	for _, header := range headers {
		ips := r.Header.Get(header)
		if ips != "" {
			return strings.TrimSpace(strings.Split(ips, ",")[0])
		}
	}
	ip := r.RemoteAddr
	colon := strings.LastIndex(ip, ":")
	if colon != -1 {
		ip = ip[:colon]
	}
	return strings.TrimSpace(ip)
}
```

### GetRequestOrigin  

```go
func GetRequestOrigin(r *http.Request) string {
	switch {
	case r.Header.Get("Host") != "":
		return r.Header.Get("Host")
	case r.Header.Get("Origin") != "":
		return r.Header.Get("Origin")
	case r.Header.Get("Referer") != "":
		return r.Header.Get("Referer")
	default:
		return "?????"
	}
}
```

### IsValidURL  

```go
// IsValidURL ...
func IsValidURL(rawurl string) bool {
	rawurl = strings.TrimSpace(rawurl) 
	parsedURL, err := url.Parse(rawurl)
	if err != nil {
		return false
	}	
	if parsedURL.Scheme != "http" && parsedURL.Scheme != "https" {
		return false
	}
	if parsedURL.Host == "" {
		return false
	}
	return true
}
```

### ExistsURL

```go
// ExistsURL ...
func ExistsURL(myUrl string) bool {
	client := http.Client{
		Timeout: 5 * time.Second,
		CheckRedirect: func(req *http.Request, via []*http.Request) error {
			return http.ErrUseLastResponse // Dont follow redirections
		},
	}

	resp, err := client.Head(myUrl)
	if err != nil {
		return false
	}
	defer resp.Body.Close()

	return resp.StatusCode >= 200 && resp.StatusCode < 400
}
```

### GetLanguage  

```go
// GetLanguage ...
func GetLanguage(r *http.Request) string {
	lang := r.Header.Get("Accept-Language")
	if lang != "" {
		langs := strings.SplitN(lang, ",", 2)
		return strings.ToLower(strings.TrimSpace(langs[0]))
	}
	return "en"
}

```

### RemoveProtocolFromURL

```go
// RemoveProtocol ...
func RemoveProtocol(url string) string {
	if strings.HasPrefix(url, "https://") {
		return url[8:]
	}
	if strings.HasPrefix(url, "http://") {
		return url[7:]
	}
	return url
}
```

### RemoveProtocolAndWWWFromURL

```go
// RemoveProtocolAndWWW ...
func RemoveProtocolAndWWWL(url string) string {
	url = RemoveProtocol(url)
	if strings.HasPrefix(url, "www.") {
		return url[4:]
	}
	return url
}
```

### Nginx return 444

```go
func close(w http.ResponseWriter, r *http.Request) {
	hijacker, ok := w.(http.Hijacker)
	if !ok {
		http.Error(w, "Server does not support hijacking", http.StatusInternalServerError)
		return
	}
	conn, _, err := hijacker.Hijack()
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	conn.Close()
	return
}
```

### Slow Response  

```go
func slowSend(w http.ResponseWriter, r *http.Request) {
	flusher, ok := w.(http.Flusher)
	if !ok {
		err := "Server does not support flusher"
		http.Error(w, err, http.StatusInternalServerError)
		return
	}

	w.Header().Set("Content-Type", "text/plain")
	fmt.Fprintln(w, "Initiating slow response...")

	for i := 0; i < 100; i++ {
		fmt.Fprintf(w, "Fragmento %d\n", i+1)
		flusher.Flush()
		time.Sleep(1 * time.Second)
	}

	fmt.Fprintln(w, "Task accomplished")
}

func holdConn(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Begin.............")
	time.Sleep(60 * time.Second)
	fmt.Fprintf(w, "Thats all")
}
```
---

## NUMBERS

### GetRandomInt  

```go
import (
	"time"

	"golang.org/x/exp/rand"
)

// RandomInt ... min and max included
func RandomInt(min, max int) int {
	r := rand.New(rand.NewSource(time.Now().UnixNano()))
	return r.Intn(max-min+1) + min
}
```

### RoundFloat64  

```go
// RoundFloat64 ... rounds float64 into integer
func RoundFloat64(num float64) int {
	if num < 0 {
		return int(num - 0.5)
	}
	return int(num + 0.5)
}
```

### RoundFloat32  

```go
// RoundFloat32 ... rounds float32 into integer
func RoundFloat32(num float32) int {
	if num < 0 {
		return int(num - 0.5)
	}
	return int(num + 0.5)
}
```

### ReverseSliceInt  

```go
// ReverseSliceInt ... [0,1,2,3,4,5] ==> [5,4,3,2,1,0]
func ReverseSliceInt(reverse []int) []int {
	for i, j := 0, len(reverse)-1; i < j; i, j = i+1, j-1 {
		reverse[i], reverse[j] = reverse[j], reverse[i]
	}
	return reverse
}
```

### TransposeMatrixInt  

```go
// TransposeMatrixInt ... rows > cols or cols > rows
// but rows.elements >= cols.elements
func TransposeMatrixInt(matrix [][]int) [][]int {
	if len(matrix) == 0 {
		return [][]int{}
	}

	result := make([][]int, len(matrix[0]))
	for i := range result {
		result[i] = make([]int, len(matrix))
	}

	for y, row := range matrix {
		for x, value := range row {
			result[x][y] = value
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

---

## STRINGS

### RemoveAllWhitespaces 

```go
import "strings"

// RemoveAllWhitespaces ...
func RemoveAllWhitespaces(str string) string {
	return strings.ReplaceAll(str, " ", "")
}
```

### ReplaceAllWhitespacesByChar  

```go
import "strings"

// ReplaceAllWhitespacesByChar ...
func ReplaceAllWhitespacesByChar(str, otherChar string) string {
	return strings.ReplaceAll(str, " ", otherChar)
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
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return [][]string{}
	}

	result := make([][]string, len(matrix[0]))
	for i := range result {
		result[i] = make([]string, len(matrix))
	}
	for y, row := range matrix {
		for x, value := range row {
			result[x][y] = value
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

---

## OS

### execCommand  

```go
import (
	"fmt"
	"os/exec"
)

func main()  {
	command := []string{"vnstat", "-i", ifinterface, "--json"}
	///fmt.Println("Command =>", command)
	chunk, err := execCommand(command)
	if err != nil {
		log.Fatal(err)
	}
	//fmt.Println(`CHUNK =>`, string(chunk))
}

func execCommand(args []string) (err error) {
	_, err = exec.Command(args[0], args[1:]...).CombinedOutput()
	if err != nil {
		fmt.Println(err)
		return err
	}
	return err
}

func execCommand(args []string) (c []byte, err error) {
	c, err = exec.Command(args[0], args[1:]...).CombinedOutput()
	if err != nil {
		return nil, err
	}
	return c, err
}

func execCommand(comm string) {
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
func ParseStringToTime(start string) (time.Time, error) {
	formats := []string{
		time.RFC3339,
		"2006-01-02T15:04:05",
		"2006-01-02",
	}

	for _, layout := range formats {
		t, err := time.Parse(layout, start)
		if err == nil {
			return t, nil
		}
	}

	fail := fmt.Errorf("Fail parsing time: %s", start)
	return time.Time{}, fail
}
```

### OnceADayTask

```go
func main() {
	go onceADayTask(3, 10, 10)
	select {}
}

func onceADayTask(h, m, s int) {
	t := time.Now()
	n := time.Date(
		t.Year(), t.Month(), t.Day(),
		h, m, s, 0,
		t.Location(),
	)
	if n.Before(t) {
		n = n.Add(24 * time.Hour)
	}

	for {
		time.Sleep(time.Until(n))
		doSomeTask()
		n = n.Add(24 * time.Hour)
	}
}

func doSomeTask() {
	fmt.Printf("Hi: %s\n", time.Now().Format("03:04:05 PM"))
}
```

### SetInterval

```go
func main() {
	ticker := time.NewTicker(1 * time.Second)
	defer ticker.Stop()
	done := make(chan bool)
	go func() {
		for {
			select {
			case <-ticker.C:
				fmt.Println("Ticker every X * time.Second")
			case <-done:
				return
			}
		}
	}()
	time.Sleep(5 * time.Second)
	done <- true
	fmt.Println("Ticker stopped. Exiting program.")
}
```

```go
func main() {
	go interval()
	select {}
}
func interval() {
	ticker := time.NewTicker(2 * time.Second)
	defer ticker.Stop()

	for range ticker.C {
		fmt.Println("Hi Every 2 secs")
	}
}
```

---

## LOGS

### Custom Logs

```go
// main.go
/////// Custom Error Log File + Custom Info Log File /////////
iLog := createCustomInfoLogFile2(a.Conf.InfoLogFile)
mylog := createCustomErrorLogFile(a.Conf.ErrorsLogFile)
defer mylog.Close()
//////////////////////////////////////////////////////////////

// ya por donde queramos
func createCustomErrorLogFile(f string) *os.File {
	mylog,err:=os.OpenFile(f,os.O_WRONLY|os.O_CREATE|os.O_APPEND,0644)
	if err != nil {
		log.Fatalf("ERROR opening Error log file %s\n", err)
	}
	log.SetOutput(mylog)
	return mylog
}

func createCustomInfoLogFile2(f string) *log.Logger {
	infoLog,err:=os.OpenFile(f,os.O_WRONLY|os.O_CREATE|os.O_APPEND,0644)
	if err != nil {
		log.Fatalf("ERROR opening Info log file %s\n", err)
	}
	var iLog *log.Logger
	iLog = log.New(infoLog, "INFO :\t", log.Ldate|log.Ltime)
	return iLog
}
```

```go
const (
	errorLogFile = "error.log"
	infoLogFile  = "info.log"
)

func openLogFile(filename string) (*os.File, error) {
	return os.OpenFile(
		filename,
		os.O_CREATE|os.O_WRONLY|os.O_APPEND,
		0664,
	)
}

func main() {
	errorFile, err := openLogFile(errorLogFile)
	if err != nil {
		log.Fatalf("ERROR opening error log: %s: %v", errorLogFile, err)
	}
	defer errorFile.Close()

	infoFile, err := openLogFile(infoLogFile)
	if err != nil {
		log.Fatalf("ERRRO opening info log: %s: %v", infoLogFile, err)
	}
	defer infoFile.Close()

	eLog :=
		log.New(errorFile, "ERROR: ", log.Ldate|log.Ltime|log.Lshortfile)
	iLog :=
		log.New(infoFile, "INFO: ", log.Ldate|log.Ltime)

	eLog.Println("ERROR")
	iLog.Println("INFO")
}
```

### PrettyPrint Structs

```go
func prettyPrintStruct(s interface{}) {
	result, _ := json.MarshalIndent(s, "", "\t")
	fmt.Print(string(result), "\n")
}
```

---


## FLAGS

### Binarios con versiones

```go
package main

import (
	"flag"
	"fmt"
	"os"
)

var version = "0.0.0"
var when = "undefined"

func main() {
	checkFlags()
	fmt.Println("Continue...")
}

func checkFlags() {
	versionFlag := flag.Bool("v", false, "Show current version and exit")
	flag.Parse()
	switch {
	case *versionFlag:
		fmt.Printf("Version:\t: %s\n", version)
		fmt.Printf("Date   :\t: %s\n", when)
		os.Exit(0)
	}
}

/*
go build  -ldflags="
-X 'main.version=v0.2.0' 
-X 'main.when=$(date -u +%F_%T)'"

go build  -ldflags="-X 'main.when=$(date -u +%F_%T)'"

luego podemos hacer ./binary -v
*/
```

### Args + Flags + test

```makefile
#go build -ldflags="-X 'main.when=$(date -u +%F_%T)'"
DATE=$(shell date -u +%F_%T)
LDFLAGS=-ldflags "-X main.when=$(DATE)"

all: build

build:
	go build $(LDFLAGS) 

clean:
	rm binary
```

```go 
// main.go
var version = "0.0.1"
var when = ""

func main() {
	tasks := checkFlags()

	fmt.Println("USER => ", getUserName())
	fmt.Println("TASKS=", tasks)
}

func checkFlags() []string {
	if len(os.Args) > 1 && os.Args[1] == "-v" {
		versionFlag := flag.Bool("v", false, "Show Version")
		flag.Parse()
		if *versionFlag {
			fmt.Printf("Version ->\t%s\n", version)
			fmt.Printf("Date    ->\t%s\n", when)
			os.Exit(0)
		}
	}
	var result []string
	validFlags := []string{"vnstat", "www", "cloud", "mysql", "rsync"}

	for _, arg := range os.Args[1:] {
		if strings.HasPrefix(arg, "-") {
			flag := strings.TrimPrefix(arg, "-")
			for _, validFlag := range validFlags {
				if flag == validFlag {
					if !includes(result, validFlag) {
						result = append(result, validFlag)
					}
				}
			}
		}
	}
	return result
}

func getUserName() string {
	currentUser, err := user.Current()
	if err != nil {
		fmt.Println("Error:", err)
		os.Exit(0)
	}
	return currentUser.Username
}

func includes(slice []string, element string) bool {
	for _, v := range slice {
		if v == element {
			return true
		}
	}
	return false
}

// main_test.go
func TestCheckFlags(t *testing.T) {
	tests := []struct {
		name     string
		args     []string
		expected []string
	}{
		{"Test vnstat flag", []string{"cmd", "-vnstat"},
			[]string{"vnstat"}},
		{"Test www flag", []string{"cmd", "-www"},
			[]string{"www"}},
		{"Test cloud flag", []string{"cmd", "-cloud"},
			[]string{"cloud"}},
		{"Test mysql flag", []string{"cmd", "-mysql"},
			[]string{"mysql"}},
		{"Test rsync flag", []string{"cmd", "-rsync"},
			[]string{"rsync"}},
		{"Test vnstat and mysql flags",
		  []string{"cmd", "-vnstat", "-mysql"},
			[]string{"vnstat", "mysql"}},
		{"Test no flags", []string{"cmd"}, []string{}},
		{"Test repeated flags", []string{"cmd", "-www", "-www"},
			[]string{"www"}},
		{"Test unknown flag", []string{"cmd", "-unknown"},
			[]string{}},
		{"Test multiple unknown flags", 
		[]string{"cmd", "-unknown", "-anotherunknown"}, 
		[]string{}},
		{"Test multiple unknown and known flags",
		  []string{"cmd", "-www", "-unknown", "-rsync", "-anotherunk"},
			[]string{"www", "rsync"}},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			os.Args = tt.args
			flag.CommandLine = flag.NewFlagSet(os.Args[0], flag.ExitOnError)
			result := checkFlags()
			if len(result) != len(tt.expected) {
				t.Errorf("expected length %d, got %d",
				 len(tt.expected), len(result))
			}
			for i, v := range tt.expected {
				if result[i] != v {
					t.Errorf("expected %s at index %d, got %s", 
					v, i, result[i])
				}
			}
		})
	}
}


```

---

## ORGANIZACION DE CODIGO

### Compartir structs entre paquetes

```go
// main.go
package main

import (
	s "pruebas/secondarypkg"
	"time"
)

func main() {
	p := &s.Placeholder{
		Name: "FooBar",
		Date: time.Now().String(),
	}
	s.Foo(p)
}

// secondarypkg/otro.go
package secondarypkg

import "fmt"

type Placeholder struct {
	Name string
	Date string
}

func Foo(p *Placeholder) {
	fmt.Println(p.Date, p.Name)
}
```

```go
// main.go
package main

import (
	s "pruebas/paquete"
	"time"
)
func main() {
	p := s.NewPlaceHolder("FooBar", time.Now().String())
	p.Foo()
}

// secondarypkg/otro.go
package secondarypkg

import "fmt"

type Placeholder struct {
	Name string
	Date string
}

func (p *Placeholder) Foo() {
	fmt.Println(p.Date, p.Name)
}

func NewPlaceHolder(name string, date string) *Placeholder {
	return &Placeholder{
		Name: name,
		Date: date,
	}
}
```

Lo mismo usando interfaces

```go
// main.go
package main

import (
	s "pruebas/paquete"
	"time"
)

func main() {
	p := s.NewPlaceHolder("FooBar", time.Now().String())
	p.Foo()
}

// secondarypkg/otro.go
package secondarypkg

import "fmt"

type PlaceHolder interface {
	Foo()
}

type placeholder struct {
	Name string
	Date string
}

func (p *placeholder) Foo() {
	fmt.Println(p.Date, p.Name)
}

func NewPlaceHolder(name string, date string) PlaceHolder {
	return &placeholder{
		Name: name,
		Date: date,
	}
}
```

---
