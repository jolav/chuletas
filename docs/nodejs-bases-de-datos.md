
# NODEJS para BASES DE DATOS

---

## SQLITE

### driver

[better-sqlite3](https://github.com/WiseLibs/better-sqlite3)

### backups

Al activar pragma('journal_mode=WAL'), en vez de un archivo hay 3 para la DB.   
`nombre.db3`   
`nombre.db3-shm`   
`nombre.db3-wal`  

Para hacer un backup sin riesgo consolidando los tres archivos  
```sh
apt install sqlite3
sqlite3 myArchivo.db3 ".backup backup-MyArchivo.db3"
```

### Select, Replace e Insert  

```js
/* */
const options = {};

import sqlite3 from 'better-sqlite3';
const myDB = new sqlite3('./path/to/databaseFile.db3', options);
myDB.pragma('journal_mode = WAL');

const store = {

  table1: "table1",
  readTable1: function () {
    const sql = `SELECT * FROM ${this.table1}`;
    const stmt = myDB.prepare(sql);
    const rows = stmt.all();
    return rows.map(function (row) {
      return {
        server: row.server,
        data: row.data
      };
    });
  },
  saveTable1: function (req) {
    const data = req.body.data;
    const server = req.body.server;
    if (!server || !data) {
      return;
    }
    const sql = `REPLACE INTO ${this.table1} (server, data) 
    VALUES (?, ?)`;
    const stmt = myDB.prepare(sql);
    stmt.run(server, data);
  },

  table2: "table2",
  readTable2: function () {
    const sql = `SELECT * FROM "${this.table2}"`;
    const stmt = myDB.prepare(sql);
    const rows = stmt.all();
    return rows.map(function (row) {
      return {
        day: row.day,
        alexa: row.alexa,
        loc: row.loc,
        stars: row.stars,
        proxy: row.proxy,
        headers: row.headers,
        weather: row.weather,
        geoip: row.geoip,
        video2gif: row.video2gif,
        sp500: row.sp500,
        tetris: row.tetris,
        random: row.random,
      };
    });
  },
  readTodayTable2: function () {
    const today = new Date().toISOString().split('T')[0];
    const sql = `SELECT * FROM "${this.table2}" WHERE day = ?`;
    const stmt = myDB.prepare(sql);
    const rows = stmt.all(today);
    return rows[0];
  },
  saveTable2: function (data) {
    const inserts = [
      data.day,
      data.alexa,
      data.loc,
      data.stars,
      data.proxy,
      data.headers,
      data.weather,
      data.geoip,
      data.video2gif,
      data.random,
    ];
    const sql = `
    INSERT INTO "${this.table2}" 
    (day, alexa, loc, stars, proxy, headers, weather, 
    geoip, video2gif, random)
    VALUES (?, 0, 0, 0, 0, 0, 0, 0, 0, 0)
    ON CONFLICT(day) DO UPDATE SET
      alexa = ?,
      loc = ?,
      stars = ?,
      proxy = ?,
      headers = ?,
      weather = ?,
      geoip = ?,
      video2gif = ?,
      random = ?
    `;
    const stmt = myDB.prepare(sql);
    stmt.run(inserts);
  },
};

export {
  store
};

```

## MYSQL 8

### driver 

[documentacion](https://sidorares.github.io/node-mysql2/docs)  
[github](https://github.com/sidorares/node-mysql2)

## MYSQL 5 (desactualizado)

### driver

[2.17.X https://github.com/mysqljs/mysql](https://github.com/mysqljs/mysql)

`npm install --save mysql`

```js
const mysql = require('mysql');
const con = mysql.createConnection({
  host     : 'localhost' || process.env.HOST,
  user     : 'username' || process.env.USER,
  password : 'password' || process.env.PASSWORD,
  database : 'databaseName' || process.env.DB
});
```
### test

```js
function testDB () {
  console.log('Connecting ......');
  // console.log(con)
  con.connect(function (err) {
    if (err) {
      console.log('Error connecting to DB => ', err);
    } else {
      console.log('Connection OK');
    }
  });
}
```

### configurar la conexion

```js
const TABLE = process.env.TABLE;
const TABLE2 = process.env.TABLE2;

const mysql = require('mysql');
const con = mysql.createConnection({
  host: process.env.HOST,
  user: process.env.MYSQLUSER,
  password: process.env.PASSWORD,
  database: process.env.DB,
  connectTimeout: 20000, // avoid ETIMEDOUT
  acquireTimeout: 20000 // avoid ETIMEDOUT
});
```

### Mantener la conexion activa

```js
function initApp() {
  setInterval(function () {
    keepConnectionAlive();
  }, 5000);
}

function keepConnectionAlive() {
  let sql = 'SELECT 1';
  sql = mysql.format(sql);
  con.query(sql, function (err, rows) {
    if (err) {
      console.error('Error Keeping connection Alive =>', err);
      // throw err
    } else {
      //console.log('Keep connection Alive =>', rows);
    }
  });
}
```

### select

```js
function loadIP () {
  let sql = 'SELECT INET6_NTOA(ip) AS ip FROM ??;';
  const inserts = [TABLE];
  sql = mysql.format(sql, inserts);
  con.query(sql, function (err, rows) {
    if (err) {
      console.log('ERR => ', err);
    }
    console.log(rows[1].ip);
  });
}
```

### insert

```js
function insertHit (d, id, callback) {
  let sql = 'INSERT INTO ?? (ip, time) VALUE (INET6_ATON(?), ?);';
  const inserts = [TABLE, d.ip, d.time];
  sql = mysql.format(sql, inserts);
  con.query(sql, function (err, rows) {
    if (err) {
      console.log('Insert HIT error =>', err);
    // throw err
    } else {
      // console.log(rows)
      callback();
    }
  });
}
```

### update

```js
function updateDB (dbData) {
  let sql = 'UPDATE bandwith ';
  sql += 'SET data = ? ';
  sql += 'WHERE server = ?';
  const inserts = [dbData.data, dbData.server];
  sql = mysql.format(sql, inserts);
  con.query(sql, function (err) {
    if (err) {
      throw err;
    } else {
      con.end(function () {
        console.log('Update OK');
        return;
      });
    }
  });
}
```

```js
function insertHit(time) {
  let sql = 'INSERT INTO ?? (time, geoip)';
  sql += ' VALUES (?, 0)';
  sql += ` ON DUPLICATE KEY UPDATE geoip = geoip + 1;`;
  const inserts = [TABLE, time];
  sql = mysql.format(sql, inserts);
  con.query(sql, function (err, rows) {
    if (err) {
      console.error('Insert HIT error =>', err);
      // throw err
    } else {
      //console.log(rows);
    }
  });
}
```

```js
function insertHit (data) {
  let sql = 'INSERT INTO ?? (time, alexa, loc, stars, proxy, headers, 
  weather)';
  sql += ' VALUES (?, 0, 0, 0, 0, 0, 0)';
  sql += ` ON DUPLICATE KEY UPDATE ${data.service} = ${data.service} +1;`;
  const inserts = [MY_TABLE, data.time]; // , data.service, data.service]
  sql = mysql.format(sql, inserts);
  con.query(sql, function (err, rows) {
    if (err) {
      console.log('Insert HIT error =>', err);
    // throw err
    } else {
      // console.log(rows)
    }
  });
}
```

### cascada

```js
function dailyUpdate () {
  const today = new Date();
  const yesterday = lib.addDays(today, -1).toISOString().split('T')[0];
  console.log('yesterday', yesterday);
  let sql = 'SELECT count(*) as hits FROM ?? WHERE time=?';
  const inserts = [TABLE2, yesterday];
  sql = mysql.format(sql, inserts);
  con.query(sql, function (err, rows) {
    if (err) {
      console.log('ERR => ', err);
      return;
    }
    console.log(rows[0].hits);
    // UPDATE
    let sql2 = 'INSERT INTO ?? (day, day_hits) VALUE (?, ?);';
    const inserts2 = [TABLE1, yesterday, rows[0].hits];
    sql2 = mysql.format(sql2, inserts2);
    con.query(sql2, function (err, rows) {
      if (err) {
        console.log('Select origin2 ERROR =>', err);
        throw err;
      } else {
        console.log('UPDATED ', TABLE2);
        // DELETE YESTERDAY HITS
        let sql3 = 'delete FROM ?? where time=?;';
        const inserts3 = [TABLE2, yesterday];
        sql3 = mysql.format(sql3, inserts3);
        con.query(sql3, function (err, rows) {
          if (err) {
            console.log('Delete yesterday hits ERROR =>', err);
            throw err;
          } else {
            console.log('UPDATED ', TABLE2);
            con.end(function () {});
          }
        });
      }
    });
  });
}
```

### ejemplo completo

```js
// probado con mysql driver 2.12.0
const db = {
  getData: function (req, res , callback) {
    const sql = 'SELECT id, nombre FROM ??';
    const inserts = [tableName];
    sql = mysql.format(sql, inserts);
    con.query(sql, function (err, rows) {
      if (err) {
        throw err;
      } else {
        callback(rows);
      }
    });
  },
  getDataJoin: function (req, res, callback) {
    var sql = 'SELECT puesto, bidonlote, bidonorden, adhesivos.nombre AS
    adhesivo, oentrada,  operarios.nombre AS operario, fentrada ';
    sql += 'FROM registros ';
    sql += 'JOIN adhesivos ON registros.adhesivo = adhesivos.id ';
    sql += 'JOIN operarios ON registros.oentrada = operarios.id ';
    sql += 'WHERE fsalida IS NULL ';
    sql += 'ORDER BY puesto ASC;';
    var inserts = [];
    sql = mysql.format(sql, inserts);
    con.query(sql, function (err, rows) {
      if (err) {
        throw err;
      } else {
        callback(res);
      }
    });
  },
  insertData: function (req, res, callback) {
    const sql = 'INSERT INTO table (value1, value2) VALUES (?, ?)';
    const inserts = [req.body.value1, req.body.value2];
    sql = mysql.format(sql, inserts);
    con.query(sql, function (err) {
      if (err) {
        throw err;
      } else {
        console.log('Insert OK');
      }
    });
  },
  updateData: function (req, res, callback) {
    const sql = 'UPDATE records ';
    sql += 'SET field1 = ?, field2 = ? ';
    sql += 'WHERE field1 IS NULL AND field2 IS NULL AND field3 = ?';
    const inserts = [req.body.field1, new Date(), req.body.field3];
    sql = mysql.format(sql, inserts);
    con.query(sql, function (err) {
      if (err) {
        throw err;
      } else {
        console.log('Update OK');
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
```

### trucos

* **`testConnection`** - si lo ejecutas varias veces una vez da error la otra bien y asi todo el rato. Es por algo de que al ser asincrono no cierra bien. Si no usas connect ni end, solo con querys va bien  

---



