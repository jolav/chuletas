# SERVIDOR

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

Los log de baneos estan en `var/log/fail2ban.log`  

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

Contenido de brusbilis. Aqui son todos host virtuales que escuchan por el mismo puerto, el 80. Asi que el redireccionamiento se hace por nombre

El primer virtualHost que sirve brusbilis.com y www.brusbilis.com esta preparado para ejecutar paginas PHP, el resto no.

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name brusbilis.com www.brusbilis.com;

    root /var/www/html;
    index index.html index.php;

    location / {
        try_files $uri $uri/ =404;
    }
	# pass PHP scripts to FastCGI server listening on the php-fpm socket
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}

server {
    listen 80;
    listen [::]:80;

    server_name meteorHelp.brusbilis.com;

    root /var/www/meteorHelp;
    index index.html meteorHelp.html;

    location / {
            try_files $uri $uri/ =404;
    }
}

server {
    listen 80;
    listen [::]:80;

    server_name debian.brusbilis.com;

    root /var/www/debian;
    index index.html debian.html;

    location / {
            try_files $uri $uri/ =404;
    }
}

 # METEOR EN PRODUCCION
server {
    listen 80 ;
    listen [::]:80 ;

    server_name	aplicateca.brusbilis.com;

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://127.0.0.1:3000/;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

### Redireccion por IP

Redirigir acceso por direccion IP al nombre del dominio

```nginx
server {
    listen 80;

    # Listen to your server ip address
    server_name your-server-ip;

    # Redirect all traffic comming from your-server-ip to your domain
    return 301 $scheme://example.com$request_uri;
}
```

```nginx
server {
    listen 80;

    # Listen to your server ip address
    server_name 89.38.144.25;

    # Redirect all traffic comming from your-server-ip to your domain
    return 301 $scheme://brusbilis.com$request_uri;
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

### Instalacion en local

`nano /etc/nginx/nginx.conf`

```sh
# user www-data;
user brus;
```

```nginx
server {
  listen       3000;

  server_name  localhost;

  root /ruta/BrusBilis/html/plantilla;
  index index.html index.htm ;

  location / {
          ssi on;
  }
}
```

### Desactivar autoarranque nginx al inicio

`update-rc.d -f nginx disable`


### Server Side Includes

[Server Side Includes](http://nginx.org/en/docs/http/ngx_http_ssi_module.html)

[Directivas y variables](http://fortboise.org/useful/ssi-manual.html)

Para activarlas poner `ssi on` en `/etc/nginx/sites-available`

```nginx
location / {
    ssi on;
    ...
}
```

### Seguridad

* **Ocultar la firma del servidor**

```nginx
nano /etc/nginx/nginx.conf
// y añadir dentro de http
http {
  server_tokens off;
}
```

---

## MYSQL

### Instalacion

```sh
apt-get install mysql-server
mysql_install_db
mysql_secure_installation
// poner contraseña al root, luego contestar al root NO, resto ENTER
```

### Configuracion

```sh
nano /etc/mysql/my.cnf
//Asegurarse descomentar la linea para evitar al acceso desde el exterior
// Nosotros accederemos con workbench a traves de SSH
bind-address            = 127.0.0.1

mysql -u root -pContraseñaQueSea
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY
'ContraseñaQueSea' WITH GRANT OPTION;

mysql> FLUSH PRIVILEGES;

service mysql restart
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

---

## PM2

### Instalacion

Paquete de npm  
`npm install pm2 -g`

### Configuracion

Ejecutar lo siguiente como root
```sh
pm2 start main.js //pm2 levantara automaticamente main.js si se cae
pm2 startup debian //pm2 se ejecuta al inicio y levantara main.js
pm2 save
```

### Mas comandos

```sh
pm2 kill
npm remove pm2 -g
pm2 list
```

---

### UPSTART

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

### Actualizacion

Despues de actualizarse hay veces que no arranca  

```sh
/etc/init.d/webmin stop
/etc/init.d/webmin start
```

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
apt-get install letsencrypt -t jessie-backports
letsencrypt certonly
```

Ahora configurar nginx para que sirva todo por SSL port 443

```sh
nano /etc/nginx/sites-available/archivo
// No olvidar despues
cp /etc/nginx/sites-available/archivo /etc/nginx/sites-enabled/archivo
service nginx restart
```

Añadir posibilidad de peticion https

```nginx
# HTTPS server
server {
   listen 443 ssl;
   server_name brusbilis.com;
   ssl_certificate /etc/letsencrypt/live/brusbilis.com/cert.pem;
   ssl_certificate_key /etc/letsencrypt/live/brusbilis.com/privkey.pem;
   ssl_session_cache shared:SSL:1m;
   ssl_session_timeout 5m;
   ssl_ciphers HIGH:!aNULL:!MD5;
   ssl_prefer_server_ciphers on;
   location / {
      root /var/www/html;
      index index.html index.htm;
   }
}
```

Pero lo que queremos es forzar siempre a HTTPS

```nginx
server {
        listen 80;
        listen [::]:80;

        server_name brusbilis.com www.brusbilis.com;
        return 301 https://brusbilis.com$request_uri;
}
server {
        listen 80;
        listen [::]:80;
        server_name aplicateca.brusbilis.com;

        return 301 https://aplicateca.brusbilis.com$request_uri;
}
server {
    listen 80;

    # Listen to your server ip address
    server_name 89.38.144.25;

    # Redirect all traffic comming from your-server-ip to your domain
    return 301 $scheme://brusbilis.com$request_uri;
}
# HTTPS server
server {
   listen 443 ssl;
   server_name brusbilis.com;
   ssl_certificate /etc/letsencrypt/live/brusbilis.com/cert.pem;
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
}

# HTTPS server
server {
   listen 443 ssl;
   server_name aplicateca.brusbilis.com;
   ssl_certificate /etc/letsencrypt/live/aplicateca.brusbilis.com
                                                        /cert.pem;
   ssl_certificate_key /etc/letsencrypt/live/aplicateca.brusbilis.com
                                                        /privkey.pem;
   ssl_session_cache shared:SSL:1m;
   ssl_session_timeout 5m;
   ssl_ciphers HIGH:!aNULL:!MD5;
   ssl_prefer_server_ciphers on;
   location / {
        ssi on;
        try_files $uri $uri/ =404;
        root /var/www/aplicateca;
        index index.html index.htm;
   }
}
```

Daba algun problema en navegadores de android "Your connection is not private"

Al cambiar  
`ssl_certificate /etc/letsencrypt/live/snakify.org/cert.pem;`   
por  
`ssl_certificate     /etc/letsencrypt/live/snakify.org/fullchain.pem;`  
Se arregla en chrome y en Dolphin 

### Añadir subdominios

Paramos nginx  
`service nginx stop`  

`letsencrypt certonly`  

Elegimos la opcion de webroot, y ponemos el nombre del subdominio a añadir. Le
damos la ruta `/etc/letsencrypt/` y el hara sus cosas, al terminar ya estaran 
los certificados en `/etc/letsencrypt/live` y `/etc/letsencrypt/renewal`  

### Renovacion

Probar con
`letsencrypt renew --dry-run`


---














``





