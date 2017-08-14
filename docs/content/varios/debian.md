# DEBIAN
---

Para andar con ojo en las actualizaciones:

`aptitude install apt-listbugs`

---

## REPOSITORIOS

```sh
## Debian Testing
deb http://ftp.es.debian.org/debian/ testing main contrib non-free
## Debian Security
deb http://security.debian.org/ testing/updates main contrib non-free
## Debian updates
deb http://ftp.debian.org/debian/ testing-proposed-updates main contrib
                                                              non-free
## Debian Multimedia
deb http://www.deb-multimedia.org/ testing main non-free
## Dropbox
deb [arch=i386,amd64] http://linux.dropbox.com/debian/ sid main  
## Webupd8 Java
deb http://ppa.launchpad.net/webupd8team/java/ubuntu/ xenial main
## VirtualBox
deb http://download.virtualbox.org/virtualbox/debian/ xenial contrib
## Solaar
deb http://pwr.github.io/Solaar/packages/ ./
## node
# deb https://deb.nodesource.com/node_0.10/ jessie main
# deb https://deb.nodesource.com/node_0.12/ jessie main
deb https://deb.nodesource.com/node_4.x/ jessie main
deb https://deb.nodesource.com/node_6.x jessie main
# Chrome
deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main
### Insync
deb http://apt.insynchq.com/debian jessie non-free contrib
```

[Para buscar paquetes](https://tracker.debian.org/)

---

## AÑADIR TIPO DE LETRA

Copiar la fuente a `/usr/share/fonts`

`sudo fc-cache -f -v`

---

## GPG

```sh
gpg -c archivo.txt                // cifra a archivo binario
gpg -ca archivo.txt               // cifra a archivo texto
gpg --output salida.txt -d archivo.(gpg o txt)        // descifrar

// para eliminar de la memoria la clave y que la pida al descomprimir
echo RELOADAGENT | gpg-connect-agent
```

---

## DPKG

Cuando en consola un paquete deb tiene dependencias incumplidas y no se instala, justo despues del dpkg -i hacemos un -f install y todo se instala

```sh
dpkg -i paquete.deb
apt-get -f install
dpkg -r solonombreprograma // desinstalar
```

---

## LIVE SERVER

```sh
npm install -g live-server
```

Desde la terminal en el directorio del proyecto  con `live-server` lo lanza en
el puerto 8080.

[Proyecto Live-Server](https://github.com/tapio/live-server)

---

## COMANDOS UTILES

### Tamaño archivos y dir

```sh
ls -lh
du -ah /ruta
du -h -d1
```

### Limpieza

```sh
apt-get autoclean &&  apt-get autoremove && apt-get remove 
--purge `deborphan`  

dpkg -l | grep -v ^ii | awk '{print $2}' | sed '1,5d'|xargs
 dpkg --purge   
```

### Buscar procesos

```sh
pgrep nombreABuscar
ps
// luego
kill numeroProceso
```

### Find


```sh
// Buscar Archivos
find /ruta -name nombreArchivo

// Buscar directorios
find /ruta -type d -name nombreDirectorio
```

---

## ADDUSER

Crear un usuario con su carpeta home y consola para usar

`useradd -d /home/usuario -m -s /bin/bash usuario`

Para crear la contraseña del usuario

`passwd usuario`  

---

## SSH

### Instalacion

`aptitude install openssh-server openssh-client`

### Configuracion

`nano /etc/ssh/sshd_config`

```sh
// Para quitar acceso como root
# Authentication:
LoginGraceTime 120
PermitRootLogin without-password // jode mas que poner solo no
StrictModes yes

// Por seguridad
# To enable empty passwords, change to yes (NOT RECOMMENDED)
PermitEmptyPasswords no
```

`service ssh restart`

---

## RELOJ

Para configurar la hora

`dpkg-reconfigure tzdata`

Si queremos servidor propio de dar la hora instalar servidor NTP

---

## CLAMAV

### Instalacion

Antivirus para asegurarnos de que no somos conductores de virus entre maquinas
windows y por si las moscas ...

`apt-get install clamav clamav-docs clamav-daemon clamav-freshclam`

Para que escanee archivos comprimidos

`aptitude install arc arj bzip2 cabextract lzop nomarch p7zip pax tnef
unrar-free unzip zoo lha unrar`

### Configuracion

El archivo de configuracion por si queremos cambiar algo es  
`nano /etc/clamav/freshclam.conf`  
y despues hacer  
`service clamav-freshclam restart`

Uso

```sh
// Para actualizar, como root
freshclam

// Si esta bloqueado
/etc/init.d/clamav-freshclam stop //despues service clamav-freshclam start

// para escanear como usuario normal
clamscan -r /ruta
```

---

## RKHUNTER

### Instalacion

`aptitude install rkhunter`

```sh
//para actualizar, como root
rkhunter --propupd
rkhunter --update

//para usar, como root tambien
rkhunter --check
```

---

## FAIL2BAN

### Instalacion

`apt-get install fail2ban whois`  

### Configuracion

`cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`  
`nano /etc/fail2ban/jail.local`  

```sh
// Aqui van las IPs que no tendran esas restricciones
[DEFAULT]
# "ignoreip" can be an IP address, a CIDR mask or a DNS host
ignoreip = 127.0.0.1 192.168.1.0/24
bantime  = 1800
maxretry = 3
```

```sh
// Correo al que avisar ante sucesos
# Destination email address used solely for the interpolations in
# jail.{conf,local} configuration files.
destemail = root@localhost
```

```sh
// En la seccion JAILS hay que ir definiendo lo que queremos en cada
//uno de los servicios que queremos proteger con fail2ban.

[webmin]

enabled   = true
port      = 10000
filter    = webmin-auth
banaction = iptables-multiport
action    = %(action_mwl)s
logpath   = /var/log/auth.log
maxretry  = 3

[ssh]

enabled = true
port    = ssh
filter  = sshd
logpath  = /var/log/auth.log
maxretry = 3

[ssh-ddos]

enabled  = true
port     = ssh
filter   = sshd-ddos
logpath  = /var/log/auth.log
maxretry = 6
```

`service fail2ban restart`  
`service fail2ban status`

Los log de baneos estan en `/var/log/fail2ban.log`   
Tambien se puede cotillear en `/var/log/auth.log`  

---

## UFW

### Instalacion

`apt-get install ufw`  

### Configuracion

`ufw allow ssh/tcp`  
`ufw logging on`  
Antes de estp no se podia activar porque nos echa de la sesion de SSH ufw enable

`ufw allow smtp/tcp`  
`ufw allow http/tcp`  
`ufw allow https/tcp`  
`ufw allow webmin`  

`ufw status` -  Lista de reglas  
`ufw status numbered` - Lista de reglas con numeros  
`ufw delete X` -  borra la regla X  

---

## NGINX

### Instalacion

`apt-get install nginx`  

Yo uso para las web /var/www por lo tanto  
`mkdir -p /var/www/pagina1`  
`mkdir -p /var/www/pagina2`  
Hay que darle permisos al user para toda esa carpeta  
`chown -R user:user /var/www`

### Configuracion

`nano /etc/nginx/nginx.conf`  

```nginx
// Descomentar la linea
server_names_hash_bucket_size 64;
```

El archivo de configuracion se llama default y esta /etc/nginx/sites-available.
Lo borramos o lo renombramos a default.old. La copia de default que esta en /etc/nginx/sites-enabled hay que borrarla

Ahora creamos nuestro archivo/s de configuracion. Yo pongo la configuracion de todos los sitios que hay por ejemplo aqui en brusbilis.com en un solo fichero de configuracion  
`nano /etc/nginx/sites-available/brusbilis`  
Cuando este hecho  
`cp /etc/nginx/sites-available/brusbilis /etc/nginx/sites-enabled/brusbilis`  
`service nginx restart`

### Redireccion por nombre

```nginx
server {
        listen 80;
        listen [::]:80;
        server_name brusbilis.com;
        return 301 https://brusbilis.com$request_uri;
}
```

### Redireccion por IP

Redirigir acceso por direccion IP al nombre del dominio

```nginx
server {
    listen 80;
    # Listen to your server ip address
    server_name XXX.XXX.XXX.XXX;
    # Redirect all traffic comming from your-server-ip to your domain
    return 301 https://brusbilis.com$request_uri;
}
```

### Redir. 301 www a *

SIN CONFIRMAR, yo lo tengo hecho por DNS

```nginx
OPCION A
server {
    listen   80;
    server_name www.brusbilis.com;
    location /blog/rss {
      rewrite ^ http://brusbilis.com permanent;
    }
}
OPCION B
server   {
   server_name www.domain.com;
   rewrite  ^/(.*)$  http://domain.com/$1 permanent;
}
```

### nginx.conf

```nginx
http {

        ##
        # Basic Settings
        ##

        # set client body size to 50M #
        client_max_body_size 50M;

        server_tokens off;
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;

        # limit requests
        limit_req_zone  $binary_remote_addr  zone=one:10m   rate=2r/s;

        server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;
        ssl_ciphers EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##
        gzip on;
        gzip_disable "msie6";

        gzip_vary on;
        gzip_proxied any;
        gzip_comp_level 6;
        gzip_buffers 16 8k;
        gzip_http_version 1.1;
        gzip_types text/plain text/css application/json application/javascript text/xml appl$

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}
```

* **Server Side Includes**

[Server Side Includes](http://nginx.org/en/docs/http/ngx_http_ssi_module.html)

[Directivas y variables](http://fortboise.org/useful/ssi-manual.html)

Para activarlas poner `ssi on` en `/etc/nginx/sites-available`

```nginx
location / {
    ssi on;
    ...
}
```

Para las rutas ojo porque es desde la raiz del servidor web nginx para esa location

```nginx
<!-- Head -->

<!--#include virtual="/freecodecamp/plantillas/head.html" -->
<!--#include file="/plantillas/head.html" -->
<!--#include file="/freecodecamp/plantillas/head.html" -->

root /var/www/html;
try_files $uri $uri/ =404;
index index.html index.htm;
ssi on;
location /otraRuta {
     alias /var/www/otraCarpeta;
}
```

* **Ocultar la firma del servidor**

```nginx
nano /etc/nginx/nginx.conf
// y añadir dentro de http
http {
  server_tokens off;
}
```

* **Cabeceras**

ojo que algunas pueden restringir el comportamiento de las aplicaciones que tengamos  

```nginx
# Headers to be added:
add_header Strict-Transport-Security "max-age=315360000";
add_header X-Frame-Options "DENY";
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1; mode=block";
add_header Content-Security-Policy "default-src 'self'";
add_header Public-Key-Pins 'pin-sha256="base64+primary=="; 
pin-sha256="base64+backup=="; max-age=5184000; includeSubdomains; report-uri="https://www.example.com/hpkp-report"';
```

* **Limitar tamaño archivos subida**

```nginx
        client_max_body_size 50M;
```

* **Rate Limit**

```nginx
/etc/nginx/nginx.conf, aqui definimos por ejemplo la zona= one
// con 10mb de storage y un ratio de 2 requests por segundo
# limit requests
limit_req_zone  $binary_remote_addr  zone=one:10m   rate=2r/s;

//luego hay que aplicarlo, mejor en location que en server
// le ponemos un valor de burst para permitir mas juego
location /freecodecamp/6-backEnd/ {
        # requests limit
        limit_req zone=one burst=20;
}
```

* **HTTP2**

```nginx
/etc/nginx/nginx.conf usar solo cifrados muy seguros
ssl_ciphers EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:
RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;

// hay que generar una nuevas claves, lo mejor en /etc/nginx/ssl
openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048

/etc/nginx/sites-available/archivo

// http2 solo funciona con https asi que en el bloque de listen 443
listen 443 ssl http2 default_server;
listen [::]:443 ssl http2 default_server;
#server_name isipets.com;

# my cipher
ssl_dhparam  /etc/nginx/ssl/dhparam.pem;

// para mejorar rendimiento
ssl_session_cache shared:SSL:5m;
ssl_session_timeout 1h;

// para forzar siempre https subdominios incluidos
add_header Strict-Transport-Security "max-age=15768000; 
includeSubDomains";

```

* **Cache**

```nginx
// en sites-availables bloque propio
# Expires map
map $sent_http_content_type $expires {
    default                    off;
    text/html                  epoch;
    text/css                   7d;
    application/javascript     7d;
    ~image/                    max;
}
// luego en el bloque server que queramos añadir
        expires $expires;
```

* **Zona protegida**

gestionWebRethinkDB - zona protegida  
cm - aplicacion nodejs  
go - aplicacion golang

```nginx
server {
   listen 443 ssl;
   server_name sub.brusbilis.com;
   ssl_certificate /etc/letsencrypt/live/sub.brusbilis.com/fullchain.pem;
   ssl_certificate_key /etc/letsencrypt/live/sub.brusbilis.com/privkey.pem;
   ssl_session_cache shared:SSL:1m;
   ssl_session_timeout 5m;
   ssl_ciphers HIGH:!aNULL:!MD5;
   ssl_prefer_server_ciphers on;
   location / {
      ssi on;
      try_files $uri $uri/ =404;
      root /var/www/sub;
      index index.html index.htm;
   }
   location /gestionWebRethinkDB/ {
       auth_basic "Restricted";
       auth_basic_user_file /etc/nginx/.rethinkdb.pass;
       proxy_pass http://127.0.0.1:8080/;
       proxy_redirect off;
       proxy_set_header Authorization "";
   }
   location /cm/ {
	proxy_pass http://127.0.0.1:3010/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
   }
   location /go/ {
        proxy_pass http://127.0.0.1:3020/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
   }
}
```

### Custom 404

```sh
mkdir /var/www/error y ahi ponemos 404.html 404.css , js, png ...

server {
   error_page 404 /404.html;
   location = /404.html {
        root /var/www/error;
        internal;
   }
   location = /404.css {
        root /var/www/error;
   }
   location = /caution.png {
        root /var/www/error;
   }
}

Ahora desde el 404.html llamamos a los recursos con ruta absoluta
<link rel="stylesheet" type="text/css" href="/404.css">
```

### brusbilis.com 08/17

```nginx
# Expires map
map $sent_http_content_type $expires {
    default                    off;
    text/html                  epoch;
    text/css                   max;
    application/javascript     max;
    ~image/                    max;
}
server {
	listen 80;
	listen [::]:80;
	server_name www.brusbilis.com brusbilis.com;
	return 301 https://brusbilis.com$request_uri;
}
server {
    listen 80;
    # Listen to your server ip address
    server_name XXX.XXX.XXX.XXX; 
    # Redirect all traffic comming from your-server-ip to your domain
    return 301 https://brusbilis.com$request_uri;
}
server {
   listen 443 ssl http2 default_server;
   listen [::]:443 ssl http2 default_server;
   #listen 443 ssl;
   server_name brusbilis.com;
   ssl_certificate /etc/letsencrypt/live/brusbilis.com/fullchain.pem;
   ssl_certificate_key /etc/letsencrypt/live/brusbilis.com/privkey.pem;
   ssl_session_cache shared:SSL:5m;
   ssl_session_timeout 1h;
   ssl_ciphers HIGH:!aNULL:!MD5;
   ssl_prefer_server_ciphers on;
   # my cipher
   ssl_dhparam  /etc/nginx/ssl/dhparam.pem;
   # Headers to be added:
   add_header Strict-Transport-Security "max-age=15768000; includeSubDomains";
   add_header X-Frame-Options "DENY";
   add_header X-Content-Type-Options "nosniff";
   add_header X-XSS-Protection "1; mode=block";
   #add_header Content-Security-Policy "default-src 'self'";
    
   expires $expires;
   root /var/www/html;
   try_files $uri $uri/ =404;
   index index.html index.htm;
   ssi on;
   location /freecodecamp {
        alias /var/www/freecodecamp;
   }
   location /chuletas {
	alias /var/www/chuletas;
   }
   location /freecodecamp/5-api/ {
	# requests limit
	limit_req zone=one burst=20;
	proxy_pass http://127.0.0.1:3501/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
	# para pillar la ip de visitante añadir las siguientes dos lineas
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_set_header X-Forwarded-Proto $scheme;
   }
   location /freecodecamp/6-backEnd/ {	
	# requests limit
	limit_req zone=one burst=20;
        proxy_pass http://127.0.0.1:3503/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        # para pillar la ip de visitante añadir las siguientes dos lineas
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
   }
   location /freecodecamp/7-bonus2/ {
	# requests limit
	limit_req zone=one burst=20;
        proxy_pass http://127.0.0.1:3502/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        # para pillar la ip de visitante añadir las siguientes dos lineas
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
   }
}
```

---

## MYSQL

### Instalacion

```sh
Aqui con wget cogemos la version que toque 
https://dev.mysql.com/downloads/repo/apt/ 
dpkg -i el_paquete_que_hemos_bajado.deb
nano /etc/apt/sources.list.d/mysql.list // para dejarlo a nuestro gusto
apt-get update
apt-get install mysql-server
mysql_secure_installation
```

### Configuracion

```sh
nano /etc/mysql/my.cnf
// que nos manda a ...
nano /etc/mysql/mysql.conf.d/mysqld.cnf
//Asegurarse de descomentar la linea para evitar al acceso desde el exterior
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

---

## PHP-MYSQL

### Instalacion

```sh
apt-get install php5-fpm php5-mysql

nano /etc/php5/fpm/php.ini
buscar y poner esto
cgi.fix_pathinfo=0

service php5-fpm restart
```

---

## MONGODB

### Instalacion

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

### Configuracion

```sh
nano /etc/mongod.conf
//Asegurarse descomentar la linea para evitar al acceso desde el exterior
// Nosotros accederemos con robomongo a traves de SSH
bind_ip =127.0.0.1
auth = true // para poner seguridad en la bbdd

service mongod restart
las BBDD las crea en /var/lib/mongodb
```

### Robomongo  

* Para conectar con robomongo configuramos la pestaña ssh para acceder al
servidor como usuario (tenemos capado acceso ssh como root).
* En la pestaña connection ponemos localhost pues ya estamos en el servidor a traves de ssh
* La pestaña authentication tambien hay que rellenarla

### Crear usuarios en MongoDB

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

### Instalacion

```sh
nano /etc/apt/sources.list // y añadir
"deb http://download.rethinkdb.com/apt jessie main"
wget -qO- https://download.rethinkdb.com/apt/pubkey.gpg | apt-key add -
apt-get update
apt-get install rethinkdb
```

### Configuracion

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

### Web segura

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

### crear usuarios

Un usuario `admin` que no se puede corrar ya existe. Por defecto esta sin
contraseña pero se puede poner una con un update.  
La webUI siempre se conecta como admin y se salta el proceso de autenticacion  

```sql
// Insertando en la tabla del sistema `users`
r.db('rethinkdb').table('users').insert({id: 'bob', password: 'secret'})

// update to a new value or remove it by using false
r.db('rethinkdb').table('users').get('bob').update({password: false})
```

**Permisos** se almacenan en la tabla del sistema `permissions`

`read` - leer datos de las tablas  
`write` - modificar datos  
`connect` - par abrir conexiones http, por seguridad no usar  
`config` - permite hacer cosas segun el alcance  

**Alcance**

`table` - afecta solo a una tabla  
`database` - lo anterior mas crear y eliminar tablas  
`global` - lo anterior mas crear y eliminar databases  

**grant** comando para otorgar permisos  

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

**ejemplo**

la tabla test de ejemplo inicial mejor borrarla y crearla si se quiere de nuevo
pues parece que permite entrar a todo el mundo .

Si no le pones pass al admin todas las conexiones las interpreta como admin y
entra directamente  

```js
r.db("rethinkdb").table("users").get("admin").update({password: "pass"})
r.db("rethinkdb").table("users").get("user").update({password: "pass"})
r.table("users").get("usuario").update({password: "pass"})
r.db('test').grant('usuario', {read: true, write: true, cnginxonfig: true})
```

---

## PM2

### Instalacion

Paquete de npm  
`npm install pm2 -g`

### Configuracion

Para ejecutar los procesos como user y no como root  

```sh
chmod -R 777 /home/brus/.npm/.pm2
```

Ejecutar lo siguiente como user
```sh
pm2 start main.js //pm2 levantara automaticamente main.js si se cae

// esto dara un comando para ejecutar como root, le quitamos el sudo y lo
// ejecutamos como root
pm2 startup debian //pm2 se ejecuta al inicio y levantara main.js  
su -c "env PATH=$PATH:/usr/bin pm2 startup debian -u brus --hp /home/brus"
// Esto solo hace falta una vez, despues para añadir mas procesos servira con // solo pm2 save (como user)

// lo ejecuto como root y como user, hacer pruebas para descartar cual es
// COMPROBADO: como user sirve y funciona
pm2 save
```

### Mas comandos

```sh
pm2 kill // para la ejecucion pero con un reboot se activara de nuevo
npm remove pm2 -g
pm2 list
pm2 delete 7 // elimina el proceso especifico con ese id
```

Para cancelar el arranque al inicion del restart, pero se carga todos los procesos. hay que ver como se hace uno a uno
```sh
update-rc.d -f pm2-init.sh remove // como root
```

---

## FOREVER

`npm install -g forever`  

```sh
forever start app.js
```

`forever list`  
`forever stop X` - X el numero que tiene el proceso en `forever list`  

---

## UPSTART

![debian](/z-static/images/comingSoon2.jpg)

---

## WEBMIN

### Instalacion

[Descargalo en .deb aqui](http://www.webmin.com/download.html)

```sh
dpkg -i paqueteDescargado.deb
```

Si debian protesta de dependencias instala las que pida. Estas son las mas posibles:
> `perl libnet-ssleay-perl openssl libpam-runtime libio-pty-perl python
> libauthen-pam-perl libio-pty-perl apt-show-versions`  

* **Instalar modulos**

`nginx`- [https://www.justindhoffman.com/project/nginx-webmin-module](https://www.justindhoffman.com/project/nginx-webmin-module)  

Se instala desde webmin -> webmin configuration -> webmin modules  

### Actualizacion

Despues de actualizarse hay veces que no arranca  

```sh
/etc/init.d/webmin stop
/etc/init.d/webmin start
```

### Lets Encrypt

Para cambiar la contraseña desde la consola  

`/usr/share/webmin/changepass.pl /etc/webmin root NEWPASSWORD`

* **Usar webmin con Lets Encrypt**

```sh
service nginx stop
// Creamos o tenemos certificado letsencrypt del dominio a usar
cp /etc/letsencrypt/live/dominio/privkey.pem /etc/webmin/privkey.pem
cp /etc/letsencrypt/live/dominio/cert.pem    /etc/webmin/cert.pem
cp /etc/letsencrypt/live/dominio/chain.pem   /etc/webmin/chain.pem
```

Nos logueamos en webmin

![servidor](/z-static/images/debian/webmin.png)

le damos a `aplicar` y reiniciamos el servidor web `service nginx restart`   

Por alguna extraña razon va en chrome pero no en firefox donde sale  
`SSL_ERROR_RX_RECORD_TOO_LONG`

---

## PUERTOS

### Escaneo de puertos

`àpitude install nmap`

```sh
nmap localhost //puertos abiertos en local
nmap 88.88.88.88 //puertos abiertos en internet, solo escanea 1000
nmap -p x-y ip // ej nmap -p 1-65000 192.168.1.15
```

### Abrir puertos

Usar UFW

### Comprobar conectividad

Para ver si me puedo conectar a algun sitio

`nc -v 192.168.1.100 puerto`  
`nc -v brusbilis.com 80`

---


## LETS ENCRYPT

[HTTPS certbot](https://certbot.eff.org/)

```sh
apt-get install certbot 
certbot certonly
```

Ahora configurar nginx para que sirva todo por SSL port 443

### Comandos

`certbot certonly` - para crear mas.
Usar la opcion standalone y creamos todos los dominios juntos (dominio.com y www.dominio.com) que luego todo lo hace mas facil.  
Hay que parar nginx `service nginx stop` 

```sh
certbot certonly
Saving debug log to /var/log/letsencrypt/letsencrypt.log

How would you like to authenticate with the ACME CA?
-------------------------------------------------------------------------
1: Place files in webroot directory (webroot)
2: Spin up a temporary webserver (standalone)
-------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
Please enter in your domain name(s) (comma and/or space separated)  (Enter
'c' to cancel):brusbilis.com www.brusbilis.com
Obtaining a new certificate
Performing the following challenges:
tls-sni-01 challenge for brusbilis.com
tls-sni-01 challenge for www.brusbilis.com
Waiting for verification...
Cleaning up challenges
Generating key (2048 bits): /etc/letsencrypt/keys/0067_key-certbot.pem
Creating CSR: /etc/letsencrypt/csr/0067_csr-certbot.pem

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/brusbilis.com/fullchain.pem. Your cert will
   expire on 2017-11-11. To obtain a new or tweaked version of this
   certificate in the future, simply run certbot again. To
   non-interactively renew *all* of your certificates, run "certbot
   renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

`certbot delete` - para borrar un certificado 


### Renovacion

Probar con

`certbot renew --dry-run`

para realizarlo (solo funciona si esta cerca de la fecha de renovacion) con

`certbot renew`

para forzar la Actualizacion

`certbot renew --force-renew`

---


nano /etc/nginx/nginx.conf

nano /etc/nginx/sites-available/isi2

cp /etc/nginx/sites-available/isi2 /etc/nginx/sites-enabled/isi2

service nginx restart