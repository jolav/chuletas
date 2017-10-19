# BASES DE DATOS

---

## **MYSQL**

* **Instalacion**

```sh
Aqui con wget cogemos la version que toque 
https://dev.mysql.com/downloads/repo/apt/ 
dpkg -i el_paquete_que_hemos_bajado.deb
nano /etc/apt/sources.list.d/mysql.list // para dejarlo a nuestro gusto
apt-get update
apt-get install mysql-server
mysql_secure_installation
```

* **Configuracion**

```sh
nano /etc/mysql/my.cnf
// que nos manda a ...
nano /etc/mysql/mysql.conf.d/mysqld.cnf
// Descomentar la linea para evitar al acceso desde el exterior
// Nosotros accederemos con workbench a traves de SSH
bind-address            = 127.0.0.1
service mysql restart

// para crear otros usuarios , usamos % en lugar de localhost por si 
// queremos acceder desde fuera
mysql -u root -pContraseñaQueSea
mysql>CREATE USER 'userNombre'@'%' IDENTIFIED BY 'passwordQueSea';
mysql>GRANT ALL ON nombreDB.* TO 'userNombre'@'%';
mysql> FLUSH PRIVILEGES;
```

* **Ejemplo**

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

## **MONGODB v2**

OJO que todo esto es para mongodb 2

* **Instalacion**

```sh
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
nano /etc/apt/sources.list
deb http://downloads-distro.mongodb.org/repo/debian-sysvinit dist 10gen
apt-get update
apt-get install mongodb-org
```

A veces no funciona el metapaquete mongodb-org y entonces es un lio

```sh
// Con el repositorio anterior activado
apt-get install mongodb-org=2.6.10 mongodb-org-server=2.6.10
mongodb-org-shell=2.6.10 mongodb-org-mongos=2.6.10
mongodb-org-tools=2.6.10

// No hace autoarranque y hay que crearlo manualmente
mongod --auth --port puerto
// Por defecto las BBDD las manda a /data/db y hay que crearlo
mongod --dbpath /ruta/que/queramos
mongod --repair
```

* **Configuracion**

```sh
nano /etc/mongod.conf
//Asegurarse descomentar la linea para evitar al acceso desde el exterior
// Nosotros accederemos con robomongo a traves de SSH
bind_ip =127.0.0.1
auth = true // para poner seguridad en la bbdd

service mongod restart
las BBDD las crea en /var/lib/mongodb
```

* **Robomongo**

* Para conectar con robomongo configuramos la pestaña ssh para acceder al
servidor como usuario (tenemos capado acceso ssh como root).
* En la pestaña connection ponemos localhost pues ya estamos en el servidor a traves de ssh
* La pestaña authentication tambien hay que rellenarla

* **Crear usuarios en MongoDB**

```sh
$ mongo
> use admin
> db.createUser({user:"usuarioAdmin",pwd:"contraseña"
>                   ,roles:[{role:"root",db:"admin"}]})

$ mongo -u usuarioAdmin -p contraseña --authenticationDatabase admin
> use some_db
> db.createUser({user: "usuario",pwd: "contraseña",roles: ["readWrite"]})

mongod --repair //cuando se interrumpe el servicio de mongo
mongod --auth //para iniciar el servicio con la autenticacion activada
```

---

## RETHINKDB

* **Instalacion**

```sh
nano /etc/apt/sources.list // y añadir
"deb http://download.rethinkdb.com/apt jessie main"
wget -qO- https://download.rethinkdb.com/apt/pubkey.gpg | apt-key add -
apt-get update
apt-get install rethinkdb
```

* **Configuracion**

Para que arranque al inicio

```sh
cp /etc/rethinkdb/default.conf.sample
      /etc/rethinkdb/instances.d/instance1.conf
/etc/init.d/rethinkdb restart
```

Archivo configuracion:  
`nano /etc/rethinkdb/instances.d/instance1.conf`

Para levantar el servicio con el usuario rethinkdb usar  
`/etc/init.d/rethinkdb restart`

* **Web segura**

` nano /etc/nginx/sites-available/brusbilis` para añadir la nueva ruta

```nginx
# HTTPS server
server {
   listen 443 ssl;
   server_name brusbilis.com;
   ssl_certificate /etc/letsencrypt/live/brusbilis.com/fullchain.pem;
   ssl_certificate_key /etc/letsencrypt/live/brusbilis.com/privkey.pem;
   ssl_session_cache shared:SSL:1m;
   ssl_session_timeout 5m;
   ssl_ciphers HIGH:!aNULL:!MD5;
   ssl_prefer_server_ciphers on;
   location / {
        ssi on;
        try_files $uri $uri/ =404;
        root /var/www/html;
        index index.html index.htm;
   }
   location /rethinkdb-admin/ {
       auth_basic "Restricted";
       auth_basic_user_file /etc/nginx/.rethinkdb.pass;
       proxy_pass http://127.0.0.1:8080/;
       proxy_redirect off;
       proxy_set_header Authorization "";
   }
}
```

Opcion con subdirectorios

```nginx
## Sub
server {
        listen 80;
        listen [::]:80;
        server_name s.brusbilis.com;
        return 301 https://s.brusbilis.com$request_uri;
}
# HTTPS server
server {
   listen 443 ssl;
   server_name s.brusbilis.com;
   ssl_certificate /etc/letsencrypt/live/s.brusbilis.com/fullchain.pem;
   ssl_certificate_key /etc/letsencrypt/live/s.brusbilis.com/privkey.pem;
   ssl_session_cache shared:SSL:1m;
   ssl_session_timeout 5m;
   ssl_ciphers HIGH:!aNULL:!MD5;
   ssl_prefer_server_ciphers on;
   location / {
       try_files $uri $uri/ =404;
   }
   location /rethinkdb-admin/ {
       auth_basic "Restricted";
       auth_basic_user_file /etc/nginx/.rethinkdb.pass;
       proxy_pass http://127.0.0.1:8080/;
       proxy_redirect off;
       proxy_set_header Authorization "";
   }
}
```

`cp /etc/nginx/sites-available/brusbilis /etc/nginx/sites-enabled/brusbilis`


```sh
apt-get install apache2-utils
cd /etc/nginx
htpasswd -c .rethinkdb.pass <username> // <username> nombre que queramos
service nginx restart
```

Ahoya ya en el navegador
`http://brusbilis.com/rethinkdb-admin`

* **Crear usuarios**

Un usuario `admin` que no se puede corrar ya existe. Por defecto esta sin
contraseña pero se puede poner una con un update.  
La webUI siempre se conecta como admin y se salta el proceso de autenticacion  

```sql
// Insertando en la tabla del sistema `users`
r.db('rethinkdb').table('users').insert({id: 'bob', password: 'secret'})

// update to a new value or remove it by using false
r.db('rethinkdb').table('users').get('bob').update({password: false})
```

* **Permisos** se almacenan en la tabla del sistema `permissions`

`read` - leer datos de las tablas  
`write` - modificar datos  
`connect` - par abrir conexiones http, por seguridad no usar  
`config` - permite hacer cosas segun el alcance  

* **Alcance**

`table` - afecta solo a una tabla  
`database` - lo anterior mas crear y eliminar tablas  
`global` - lo anterior mas crear y eliminar databases  

* **grant** comando para otorgar permisos  

```js
// set database scope
r.db('field_notes').grant('bob',
        {read: true, write: true, config: false});

// set table scopes
r.db('field_notes').table('calendar').grant('bob',
        {write: false});
r.db('field_notes').table('supervisor_only').grant('bob',
        {read: false, write: false});
```

* **ejemplo**

la tabla test de ejemplo inicial mejor borrarla y crearla si se quiere de nuevo
pues parece que permite entrar a todo el mundo .

Si no le pones pass al admin todas las conexiones las interpreta como admin y
entra directamente  

```js
r.db("rethinkdb").table("users").get("admin").update({password: "pass"})
r.db("rethinkdb").table("users").get("user").update({password: "pass"})
r.table("users").get("usuario").update({password: "pass"})
r.db('test').grant('usuario', {read: true, write: true, 
  cnginxonfig: true})
```

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

## **SQLITE**

* **Instalacion**

* ****

* ****

---