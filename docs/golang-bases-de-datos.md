# GOLANG para BASES DE DATOS 

---

## MYSQL

### driver

[go-sql-driver](https://github.com/go-sql-driver/mysql)

`go get -u github.com/go-sql-driver/mysql`


### Example 1

```go
import (
	"database/sql"
	"fmt"
	"log"
)

type myDB struct{}

var db1 *sql.DB
var db2 *sql.DB
var mydb1 myDB
var mydb2 myDB

func (mydb1 *myDB) initDB1() {
  connPath1 := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s", c.Mysql.User1,
  c.Mysql.Password1, c.Mysql.Host1, c.Mysql.Port1, c.Mysql.Db1)
	//fmt.Println(connPath1)
	var err error
	db1, err = sql.Open("mysql", connPath1)
	if err != nil {
		log.Fatal(err)
	} else {
		fmt.Println(`codetabs-hits sql.Open() => OK`)
	}
}

func (mydb2 *myDB) initDB2() {
  connPath2 := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s", c.Mysql.User2, 
  c.Mysql.Password2, c.Mysql.Host2, c.Mysql.Port2, c.Mysql.Db2)
	//fmt.Println(connPath2)
	var err error
	db2, err = sql.Open("mysql", connPath2)
	if err != nil {
		log.Fatal(err)
	} else {
		fmt.Println(`servers-bandwith sql.Open() => OK`)
	}
}

func (mydb1 *myDB) getCodetabsFromDB() {
  sql := "SELECT time,alexa,loc,stars,proxy,headers,weather
  ,geoip FROM `daily-hits`"
	stmt, err := db1.Prepare(sql)
	if err != nil {
		log.Fatal(err)
	}
	defer stmt.Close()
	rows, err := stmt.Query()
	if err != nil {
		log.Fatal(err)
	}
	defer rows.Close()
	statsCT = codetabsStats{}
	var i codetabsDayStats
	for rows.Next() {
    err := rows.Scan(&i.Time, &i.Alexa, &i.Loc, &i.Stars, &i.Proxy, 
    &i.Headers, &i.Weather, &i.Geoip, &i.Video2gif)
		if err != nil {
			log.Fatal(err)
		}
		statsCT = append(statsCT, i)
	}
}

func (mydb1 *myDB) getGeoipFromDB() {
	sql := "SELECT time,geoip FROM `geoip-hits`"
	stmt, err := db1.Prepare(sql)
	if err != nil {
		log.Fatal(err)
	}
	defer stmt.Close()
	rows, err := stmt.Query()
	if err != nil {
		log.Fatal(err)
	}
	defer rows.Close()
	statsIP = geoipStats{}
	var i geoipDayStats
	for rows.Next() {
		err := rows.Scan(&i.Time, &i.Hits)
		if err != nil {
			log.Fatal(err)
		}
		statsIP = append(statsIP, i)
	}
}

func (mydb2 *myDB) getBandwithFromDB(server string) {
	//fmt.Println(`SERVER =`, server)
	sql := "SELECT data FROM `bandwith` where server = ?"
	stmt, err := db2.Prepare(sql)
	//fmt.Println(stmt, err)
	if err != nil {
		log.Fatal(err)
		return
	}
	defer stmt.Close()

	err = stmt.QueryRow(server).Scan(&data)
	if err != nil {
		log.Fatal(err)
	}
}
```

### Example2

```go
import (
	"database/sql"
	"fmt"
	"log"
)

type configuration struct {
	Mysql struct {
		Host     string `json:"host"`
		Port     int    `json:"port"`
		Db       string `json:"db"`
		User     string `json:"user"`
		Password string `json:"password"`
	} `json:"mysql"`
}

type myDB struct{}

var db *sql.DB
var c configuration
var mydb myDB

func (mydb *myDB) initDB() {
	loadConfig()
  connPath := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s", c.Mysql.User, 
  c.Mysql.Password, c.Mysql.Host, c.Mysql.Port, c.Mysql.Db)
	//fmt.Println(connPath)
	var err error
	db, err = sql.Open("mysql", connPath)
	if err != nil {
		log.Fatal(err)
	} else {
		fmt.Println(`vnstat sql.Open() => OK`)
	}
}

func (mydb *myDB) updateDB(data []byte) {
	sql := fmt.Sprintf("UPDATE %s SET data = ? WHERE server = ?", table)
	stmt, err := db.Prepare(sql)
	if err != nil {
		log.Fatal(err)
	}
	defer stmt.Close()
	_, err = stmt.Exec(data, server)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(`Update OK`)
}
```

### Example3

```go
import (
	"database/sql"
	"fmt"
	"log"

	lib "../_lib"

	// mysql driver
	_ "github.com/go-sql-driver/mysql"
)

type configuration struct {
	Mysql struct {
		Host     string `json:"host"`
		Port     int    `json:"port"`
		Db       string `json:"db"`
		Table    string `json:"table"`
		User     string `json:"user"`
		Password string `json:"password"`
	} `json:"mysql"`
}

type myDB struct{}

var db *sql.DB
var c configuration

// MYDB ...
var MYDB myDB

func (MYDB *myDB) InitDB() {
	lib.LoadConfig(mysqljson, &c)
  connPath := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s", c.Mysql.User,
  c.Mysql.Password, c.Mysql.Host, c.Mysql.Port, c.Mysql.Db)
	//fmt.Println(connPath)
	var err error
	db, err = sql.Open("mysql", connPath)
	if err != nil {
		log.Printf("ERROR 1 DB %s\n", err)
	} else {
		log.Printf("INFO DB %s sql.Open() => OK\n", c.Mysql.Db)
	}
}

func (MYDB *myDB) InsertHit(service string, datenow string) {
  sql := fmt.Sprintf("INSERT INTO `%s` (time, alexa, loc, stars,
  proxy, headers, weather, video2gif) VALUES ('%s', 0, 0, 0, 0, 0, 0, 0)
  ON DUPLICATE KEY UPDATE %s = %s + 1;", c.Mysql.Table, datenow, 
  service, service)
	stmt, err := db.Prepare(sql)
	if err != nil {
		log.Printf("ERROR 2 DB %s\n", err)
		return
	}
	defer stmt.Close()
	_, err = stmt.Exec()
	if err != nil {
		log.Printf("ERROR 3 DB %s\n", err)
	}
}
```
