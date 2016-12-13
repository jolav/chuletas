# BASES DE DATOS

---

## MYSQL

```javascript
var secret = require('./../secret.json');
var mysql = require('mysql');

var db = {
  getDatos: function (req, res , callback) {
    var sql = 'SELECT bla bala';
    var inserts = [];
    sql = mysql.format(sql, inserts);
    con.query(sql, function (err, rows) {
      if (err) {
        throw err;
      } else {
        res.rows = rows;
        callback(res);
      }
    });
  },
  insertarDatos: function (req, res, callback) {
    var sql = 'INSERT INTO tabla (valor1, valor2) VALUES (?, ?)';
    var inserts = [req.body.valor1, req.body.valor2];
    sql = mysql.format(sql, inserts);
    con.query(sql, function (err) {
      if (err) {
        throw err;
      } else {
        console.log('Registro GUARDADO ...........');
        res.status(200).send('OK');
      }
    });
  },
  actualizarDatos: function (req, res, callback) {
    var sql = 'UPDATE registros ';
    sql += 'SET campo1 = ?, campo2 = ? ';
    sql += 'WHERE campo1 IS NULL AND campo2 IS NULL AND campo3 = ?';
    var inserts = [req.body.campo1, new Date(), req.body.campo3];
    sql = mysql.format(sql, inserts);
    con.query(sql, function (err) {
      if (err) {
        throw err;
      } else {
        console.log('Registro MODIFICADO ...........');
        res.status(200).send('OK');
      }
    });
  },
  testConnection: function (req, res, callback) {
    console.log('Connecting ......')
    con.connect(function (err) {
      if (err) {
        console.log('Error connecting to DB')
        res.status = false;
      } else {
        console.log('Connection OK')
        res.status = true;
      }
      con.end(function () {});
      callback(res);
    });
  }
};

var con = mysql.createConnection({
  host: secret.mysql.host,
  user: secret.mysql.user,
  password: secret.mysql.password,
  database: secret.mysql.db
});

module.exports = db;
```
`testConnection` - si lo ejecutas varias veces una vez da error la otra bien y asi todo el rato. Es por algo de que al ser asincrono no cierra bien. Si no usas connect ni end, solo con querys va bien  

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
