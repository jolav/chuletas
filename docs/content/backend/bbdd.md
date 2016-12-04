# BASES DE DATOS

---

## MYSQL

![bbdd](/z-static/images/comingSoon2.jpg)

---

## RETHINKDB

`npm install--save rethinkdb`  

* **Conexion**

```js
var config = require('./config.json');
var r = require('rethinkdb');

r.connect(config.rethinkdb)
  .then(function (conn) {
    console.log(conn);
  })
  .error(function (error) {
    console.log(error.message);
  });
```

```json
{
	"rethinkdb": {
		"host": "dominio.com",
		"port": 5555,
		"db": "test",
        "username" : "userName",
        "password" : "userPassword"
    },
	"express": {
		"port": 3000
	}
}
```

---

## REDIS

[redis](http://redis.io)

### instalacion

[descargar de aqui](http://redis.io/download)  

```sh
tar xzf redis-x.tar.gz
cd redis-x
make
```

---

## SQLITE

![bbdd](/z-static/images/comingSoon2.jpg)

---

## POUCH DB

[PouchDB](https://pouchdb.com/)


---
