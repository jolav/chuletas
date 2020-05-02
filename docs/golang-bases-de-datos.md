# GOLANG para BASES DE DATOS 

---

## MYSQL

### driver

[go-sql-driver](https://github.com/go-sql-driver/mysql)

`go get -u github.com/go-sql-driver/mysql`


### Crear conexion a la BBDD

```go
package store

import (
	"database/sql"
	"fmt"

	// mysql driver
	_ "github.com/go-sql-driver/mysql"
)

type configDB struct {
	DatabaseType string `json:"databaseType"`
	Host         string `json:"host"`
	Port         int    `json:"port"`
	DB           string `json:"db"`
	User         string `json:"user"`
	Password     string `json:"password"`
}

// DB ...
type DB struct {
	*sql.DB
}

// NewDB ...
func NewDB(mode string) (*DB, error) {
	var c configDB
	loadConfigJSON(&c)
	setDBConnConfig(mode, &c)
	connPath := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?parseTime=true",
		c.User,
		c.Password,
		c.Host,
		c.Port,
		c.DB,
	)
	//fmt.Println("CONNPATH => ", connPath)
	db, err := sql.Open(c.DatabaseType, connPath)
	if err != nil {
		return nil, err
	}
	err = db.Ping()
	if err != nil {
		return nil, err
	}
	return &DB{db}, nil
}
```

```go
// lo llamo desde otro paquete
package login

import (
	store "casServer/login/store"
)

var loginDB *store.DB

func init() {
	db, err := store.NewDB(checkAppConfMode())
	if err != nil {
		log.Fatal("Error connecting DataBase => ", err)
	}
	loginDB = db
	//users, _ := loginDB.AccountList()
	//fmt.Println(len(users))

}
```

### Select

```go
func (loginDB *DB) Account(name string) (*User, error) {
	//fmt.Println("SEARCHING ...", name)
	u := new(User)
	query := fmt.Sprintf(`
		SELECT
			*
		FROM %s
		WHERE nickID=?
		`,
		usersTable,
	)
	//fmt.Println(query)
	stmt, err := loginDB.Prepare(query)
	if err != nil {
		log.Printf("ERROR 1 DB %s\n", err)
		return u, err
	}
	defer stmt.Close()
	err = stmt.QueryRow(name).Scan(
		&u.NickID,
		&u.Nick,
		&u.PassHashed,
		&u.Email,
		&u.Verified,
		&u.Logo,
		&u.SecretQuest,
		&u.SecretHashed,
		&u.CreatedAt,
		&u.LastSeen,
		&u.Online)
	if err != nil {
		if err == sql.ErrNoRows { // no result
			//log.Println("NO RESULT", u)
			return u, nil
		}
		log.Printf("ERROR 2 DB %s\n", err)
		return nil, err
	}
	return u, nil
}
```

```go
func (loginDB *DB) AccountList() ([]*User, error) {
	query := fmt.Sprintf(`
		SELECT
			nickID
		FROM %s
		`,
		usersTable,
	)
	stmt, err := loginDB.Prepare(query)
	if err != nil {
		return nil, err
	}
	defer stmt.Close()

	rows, err := stmt.Query()
	if err != nil {
		return nil, err
	}
	defer rows.Close()
	users := make([]*User, 0)
	for rows.Next() {
		user := new(User)
		err := rows.Scan(
			&user.NickID,
		)
		if err != nil {
			return nil, err
		}
		users = append(users, user)
	}
	return users, nil
}
```

```go
func (loginDB *DB) UserSession(usernameID string) (*Session, error) {
	s := NewSession()
	query := fmt.Sprintf(`
		SELECT
			*
		FROM %s
		WHERE nickID=?
		`,
		sessionsTable,
	)
	stmt, err := loginDB.Prepare(query)
	if err != nil {
		log.Printf("ERROR 3 DB SESSIONS %s\n", err)
		return s, err
	}
	defer stmt.Close()
	err = stmt.QueryRow(usernameID).Scan(
		&s.NickID,
		&s.SessionID,
		&s.Expires,
	)
	if err != nil {
		if err == sql.ErrNoRows { // no result
			//log.Println("NO RESULT", s)
			return s, nil
		}
		log.Printf("ERROR 4 DB SESSIONS %s\n", err)
		return nil, err
	}
	return s, nil
}
```


### Insert

```go
func (loginDB *DB) NewAccount(u *User) error {
	//fmt.Println("USER => ", u)
	query := fmt.Sprintf(`
		INSERT
		INTO %s
		(
			nickID, nick, passHashed, email, verified, logo, 
			secretQuest, secretHashed, createdAt, lastSeen, online
		)
		values (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
		`,
		usersTable,
	)
	stmt, err := loginDB.Prepare(query)
	//fmt.Println(query)
	if err != nil {
		log.Printf("ERROR 3 DB USERS %s -> %s\n", u.Nick, err)
		return err
	}
	defer stmt.Close()
	_, err = stmt.Exec(
		u.NickID,
		u.Nick,
		u.PassHashed,
		u.Email,
		u.Verified,
		u.Logo,
		u.SecretQuest,
		u.SecretHashed,
		u.CreatedAt,
		u.LastSeen,
		u.Online,
	)
	if err != nil { 
		log.Printf("ERROR 4 DB USERS %s -> %s\n", u.Nick, err)
		return err
	}
	return nil
}
```

```go
func (loginDB *DB) SaveSession(s *Session) error {
	query := fmt.Sprintf(`
		INSERT 
		INTO %s 
		(
			nickID, sessionID, expires
		) 
		values (?, ?, ?)
		ON DUPLICATE KEY UPDATE sessionID=? , expires= ?
		`,
		sessionsTable)
	stmt, err := loginDB.Prepare(query)
	if err != nil {
		log.Printf("ERROR 1 DB SESSIONS %s -> %s\n", s.NickID, err)
		return err
	}
	defer stmt.Close()
	_, err = stmt.Exec(
		s.NickID,
		s.SessionID,
		s.Expires,
		s.SessionID,
		s.Expires,
	)
	if err != nil {
		log.Printf("ERROR 2 DB SESSIONS %s -> %s\n", s.NickID, err)
		return err
	}
	return nil
}
```

### Update

```go

```

### Delete

```go
func (loginDB *DB) DeleteSession(usernameID string) error {
	query := fmt.Sprintf(`
		DELETE 
		FROM %s 
		WHERE nickID = ?
		`,
		sessionsTable)
	stmt, err := loginDB.Prepare(query)
	if err != nil {
		log.Printf("ERROR 5 DB SESSIONS %s -> %s\n", usernameID, err)
		return err
	}
	defer stmt.Close()
	_, err = stmt.Exec(
		usernameID,
	)
	if err != nil {
		log.Printf("ERROR 6 DB SESSIONS %s -> %s\n", usernameID, err)
		return err
	}
	return nil
}
```

