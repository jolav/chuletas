# DEBIAN 

---

## **TESTING**

Para andar con ojo en las actualizaciones instalar  
`aptitude install apt-listbugs`

<code>[Buscador de paquetes en debian](https://tracker.debian.org/)</code>

* **Repositorios** /etc/apt/sources.list

```sh
## Debian Testing
deb http://ftp.de.debian.org/debian/ testing main contrib non-free
## Debian Security
deb http://security.debian.org/ testing/updates main contrib non-free
## Debian updates
deb http://ftp.debian.org/debian/ testing-proposed-updates main contrib
                                                              non-free
## Debian Multimedia
deb http://www.deb-multimedia.org/ testing main non-free
## Dropbox
deb [arch=i386,amd64] http://linux.dropbox.com/debian/ sid main 
## node
deb https://deb.nodesource.com/node_6.x/ stretch main
deb https://deb.nodesource.com/node_8.x/ stretch main
# Chrome
deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main
### Insync
deb http://apt.insynchq.com/debian stretch non-free contrib
```

---

## **COMANDOS**

* **Añadir tipo de fuente**

`fc-cache -f -v` - tras haber copiado la fuente a `/usr/share/fonts`  

* **gpg**

`gpg -c file.txt` - cifra a archivo binario   
`gpg -ca file.txt` - cifra a archivo de texto  
`gpg --output output.txt -d file.(gpg or txt)` - descifrar  

`echo RELOADAGENT | gpg-connect-agent` - para eliminar de la memoria la clave y que la pida al descomprimir


* **dpkg**

Cuando en consola un paquete deb tiene dependencias incumplidas y no se instala, justo despues del dpkg -i hacemos un -f install y todo se instala

`dpkg -i package.deb` -   
`apt-get -f install` -  
`dpkg -r onlyThaName` - desinstalar   

* **Tamaño de directorios y archivos**

`ls -lh`
`du -ah /path`
`du -h -d1`

* **Limpieza**

```sh
apt-get autoclean &&  apt-get autoremove &&
apt-get remove --purge `deborphan`
```

`dpkg -l | grep -v ^ii | awk '{print $2}' | sed '1,5d'|xargs
 dpkg --purge`

* **Procesos** 

`pgrep processName` - buscar    
`ps` - listar todos los procesos  
`kill processNumber` - matar proceso  
`fg` - continuar el proceso que estaba detenido  

* **Buscar**

`find /path -name fileName` - Buscar archivo    
`find /path -type d -name dirName` - Buscar directorio  

* **Crear usuario**

`useradd -d /home/username -m -s /bin/bash username` - Crea un usuario con su carpeta home y consola para usar  
`passwd username` -  Para crear la contraseña del usuario     

* **Tiempo**

`dpkg-reconfigure tzdata` - Para configurar la hora  
Si queremos servidor propio de dar la hora instalar servidor NTP  

---

## **SECURITY**

###  SSH

`aptitude install openssh-server openssh-client`
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

### CLAMAV

Antivirus para asegurarnos de que no somos conductores de virus entre maquinas windows y por si las moscas ...  
`apt-get install clamav clamav-docs clamav-daemon clamav-freshclam`  

`aptitude install arc arj bzip2 cabextract lzop nomarch p7zip pax tnef
unrar-free unzip zoo lha unrar` - Para que escanee archivos comprimidos.  

`nano /etc/clamav/freshclam.conf` - El archivo de configuracion por si queremos cambiar algo.  
`service clamav-freshclam restart` - hacerlo despues para carar la nueva configuracion.  

```sh
// Para actualizar, como root
freshclam

// Si esta bloqueado
/etc/init.d/clamav-freshclam stop //despues service clamav-freshclam start

// para escanear como usuario normal
clamscan -r /ruta
```

### RKHUNTER

`aptitude install rkhunter` - Para instalarlo  

```sh
//para actualizar, como root
rkhunter --propupd
rkhunter --update

//para usar, como root tambien
rkhunter --check
```

### FAIL2BAN

`apt-get install fail2ban whois`  
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

Los logs de baneos estan en `/var/log/fail2ban.log`  
Tambien se puede cotillear por `/var/log/auth.log`   

### UFW

Instalar - `apt-get install ufw`  

`ufw allow ssh/tcp`  
`ufw logging on`  
Antes de esto no se podia activar porque nos echa de la sesion de SSH.  
Ahora si `ufw enable`

`ufw allow smtp/tcp`  
`ufw allow http/tcp`  
`ufw allow https/tcp`  
`ufw allow webmin`  

`ufw status - Lista de reglas`  
`ufw status numbered - Lista de reglas con numeros`  
`ufw delete X - borra la regla X`   

---

## **PM2**

* **Instalacion**
  
Como paquete de npm - `npm install pm2 -g`  
Para desinstalar - `npm remove pm2 -g`  

* **Configuracion**

[DOCS](http://pm2.keymetrics.io/docs/usage/quick-start/)

`pm2 startup` como usuario y seguir instrucciones para hacer que pm2 se ejecute en los reinicios sin necesitar ser root  
`pm2 unstartup` - para desmontar el chiringuito  

**NO SIEMPRE** Para ejecutar los procesos como user y no como root   
`chmod -R 777 /home/brus/.pm2` 

* **Comandos**

```sh
pm2 start app.js  
pm2 kill // para la ejecucion pero con un reboot se activara de nuevo
pm2 list
pm2 stop all|number
pm2 restart all|number
pm2 delete 7 // elimina el proceso especifico con ese id
pm2 save // salva la lista de procesos en ese momento
```

---

## **SYSTEMD**

`systemctl status servidorGO` nos da informacion del servicio

```sh  
// Crear service
nano /etc/systemd/servidorGO.service  
cp servidorGO.service /etc/systemd/system 
systemcl enable servidorGO.service 
service servidorGO.service start
```

```sh
// Borrar service
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
systemctl daemon-reload
systemctl reset-failed
```

```sh
[Unit]
Description= Descripcion de la tarea

[Service]
User=brus
Group=www-data
Restart=on-failure   
WorkingDirectory=/var/www/path/to/binary/folder
ExecStart=/var/www/path/to/binary/folder/binaryName

[Install]
WantedBy=multi-user.target
```

* **Comandos**

`service name status`  
`service name start`  

`systemctl enable name.service`  
`systemctl disable name.service`   
`systemctl start name.service`  
`systemctl stop name.service`  
`systemctl restart name.service`  
`systemctl status name.service`  
`systemctl reload name.service` 

```sh
// Archivos que tiene que terminar habiendo
/etc/systemd/servidorGO.service
/etc/systemd/system/servidorGO.service
/etc/systemd/system/multi-user.target.wants/servidorGO.service
/sys/fs/cgroup/systemd/system.slice/servidorGO.service
``` 

---

## **NGINX**

* **Instalacion**

`apt-get install nginx`  

`chown -R user:user /var/www` - Para darle permisos al usuario para toda esa carpeta  
`mkdir -p /var/www/site1`  
`mkdir -p /var/www/site2`  

### CONFIGURACION

`nano /etc/nginx/nginx.conf`  

El archivo de configuracion se llama default y esta /etc/nginx/sites-available. Lo borramos o lo renombramos a default.old. La copia de default que esta en /etc/nginx/sites-enabled hay que borrarla

Ahora creamos nuestro archivo/s de configuracion.  
`nano /etc/nginx/sites-available/domain`    
`cp /etc/nginx/sites-available/domain /etc/nginx/sites-enabled/domain`   
`service nginx restart`


#### server side includes

[Server Side Includes](http://nginx.org/en/docs/http/ngx_http_ssi_module.html)  
[Directives and variables](http://fortboise.org/useful/ssi-manual.html)

`ssi on` - `/etc/nginx/sites-available`

Para las rutas ojo porque es desde la raiz del servidor web nginx para esa location  

```nginx
<!-- Head -->
<!--#include file="/_public/templates/head.html" -->
```

#### ocultar la firma del servidor

```nginx
nano /etc/nginx/nginx.conf
http {
  server_tokens off;
}
```

#### headers

ojo que algunas pueden restringir el comportamiento de las aplicaciones que tengamos 

```nginx
# Headers to be added:
add_header Strict-Transport-Security "max-age=15768000; 
  includeSubDomains" always;
add_header X-Frame-Options "DENY";
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1; mode=block";
add_header Content-Security-Policy "default-src 'self'";
```

#### limitar tamaño archivos subida

```nginx
location /count-loc/ {
  # set client body size to 10M
  client_max_body_size 10M;
}
```

#### rate Limit

```nginx
/etc/nginx/nginx.conf, aqui definimos por ejemplo la zona=one
# limit requests
limit_req_zone  $binary_remote_addr  zone=one:10m   rate=2r/s;

//luego hay que aplicarlo, mejor en location que en server
/etc/nginx/sites-available/domain
location /count-loc/ {
        # requests limit
        limit_req zone=one burst=20;
}
```

#### http2

```nginx
/etc/nginx/nginx.conf usar solo cifrados muy seguros
ssl_ciphers EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:
RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;

// hay que generar una nuevas claves, lo mejor en /etc/nginx/ssl
openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048

cd /etc/nginx/sites-available/domain

// http2 solo funciona con https asi que en el bloque de listen 443
listen 443 ssl http2 default_server;
listen [::]:443 ssl http2 default_server;

# my cipher
ssl_dhparam  /etc/nginx/ssl/dhparam.pem;

// para mejorar rendimiento
ssl_session_cache shared:SSL:5m;
ssl_session_timeout 1h;
```

#### cache

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

#### zona protegida

private - zona protegida

```sh
// create password file
apt-get install apache2-utils
cd /etc/nginx
htpasswd -c .rethinkdb.pass <username> // will ask for password
service nginx restart
```

```nginx
server {
   listen 443 ssl;
   server_name sub.domain.com;
   location / {
      ...
   }
   location /private/ {
      auth_basic "Restricted";
      auth_basic_user_file /etc/nginx/.rethinkdb.pass;
      ... root or proxy for this location
   }
}
```

#### custom 404

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

In 404.html we call assets using absolute path
<link rel="stylesheet" type="text/css" href="/404.css">
```


### NGINX.CONF

**updated 2017-09-25**

```nginx
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
  worker_connections 768;
  # multi_accept on;
}

http {

  ##
  # Basic Settings
  ##

  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 65;
  types_hash_max_size 2048;
  server_tokens off;

  # limit requests
  limit_req_zone  $binary_remote_addr  zone=one:10m   rate=5r/s;
  limit_req_zone  $binary_remote_addr  zone=two:10m   rate=1r/s;
  limit_req_zone  $binary_remote_addr  zone=three:10m   rate=12r/m;

  server_names_hash_bucket_size 64;
  # server_name_in_redirect off;

  include /etc/nginx/mime.types;
  default_type application/octet-stream;

  # set client body size to 5M #
  client_max_body_size 5M;

  ##
  # SSL Settings
  ##

  ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
  ssl_prefer_server_ciphers on;
  ssl_ciphers EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:
  RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
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
  gzip_types text/plain text/css application/json application/javascript
   text/xml application/xml application/xm$

  ##
  # Virtual Host Configs
  ##

  include /etc/nginx/conf.d/*.conf;
  include /etc/nginx/sites-enabled/*;
}
```

### DOMAIN.CONF

**updated 2017-09-25**

```nginx
  ssl_certificate /etc/letsencrypt/live/domain.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/domain.com/privkey.pem;
  ssl_session_cache shared:SSL:10m;
  ssl_session_timeout 1h;
  ssl_ciphers HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers on;

  # my cipher
  ssl_dhparam  /etc/nginx/ssl/dhparam.pem;

  # server-side-includes
  ssi on;

  # Headers to be added:
  add_header Strict-Transport-Security "max-age=15768000; 
  includeSubDomains" always;
  add_header X-Frame-Options "DENY";
  add_header X-Content-Type-Options "nosniff";
  add_header X-XSS-Protection "1; mode=block";
  #add_header Content-Security-Policy "default-src 'self'";

  #cache
  # set client body size to 4M #
  # client_max_body_size 4M;
  expires $expires;

  # custom error
  error_page 404 /404.html;
  location = /404.html {
          root /var/www/domain/_error;
          internal;
  }
  location = /404.css {
          root /var/www/domain/_error;
  }
  location = /ct24r.png {
          root /var/www/domain/_error;
  }
  location = /ct48r.png {
          root /var/www/domain/_error;
  }

  location /_public {
          # limit_req zone=one burst=50;
          alias /var/www/domain/_public;
  }
  location /_public-fcc {
          # limit_req zone=one burst=50;
          alias /var/www/domain/_public-fcc;
  }
```

### SITES/DOMAIN

**updated 2017-09-25**

```nginx
# Expires map
map $sent_http_content_type $expires {
  default                    off;
  text/html                  epoch;
  text/css                   7d;
  application/javascript     7d;
  ~image/                    max;
}
server {
  listen 80;
  # Listen to your server ip address
  server_name 212.24.108.85;
  # Redirect all traffic comming from your-server-ip to your domain
  return 301 https://domain.com$request_uri;
}
server {
  listen 80;
  listen [::]:80;
  server_name domain.com www.domain.com;
  return 301 https://domain.com$request_uri;
  server_name *.domain.com;
  return 301 https://*.domain.com$request_uri;
}
server {
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name www.domain.com;
  ssl_certificate /etc/letsencrypt/live/domain.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/domain.com/privkey.pem;
  return 301 https://domain.com$request_uri;
}
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name domain.com;

  include /etc/nginx/sites-available/domain.conf;

  # enable CORS
  # add_header Access-Control-Allow-Origin '*';

  location / {
    root /var/www/domain/html;
    index index.html;
	}
  location /freecodecamp {
          root /var/www/domain;
          index index.html;
  }
  location /notes {
          root /var/www/domain;
          index index.html;
  }
}
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name api.domain.com;

  include /etc/nginx/sites-available/domain.conf;

  # enable CORS
  add_header 'Access-Control-Allow-Origin' '*' always;

  location /github-stars/ {
    # requests limit
    limit_req zone=three ;#burst=50;
    proxy_pass http://127.0.0.1:3501/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    # Para coger ip de visitante añadir las siguientes dos lineas
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
  location /count-loc/ {
    # requests limit
    limit_req zone=three ;#burst=50;
    # set client body size to 10M 
    client_max_body_size 10M;
    proxy_pass http://127.0.0.1:3502/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    # Para coger ip de visitante añadir las siguientes dos lineas
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
  location /game/ {
    # requests limit
    limit_req zone=three ;#burst=50;
    proxy_pass http://127.0.0.1:3503/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    # Para coger ip de visitante añadir las siguientes dos lineas
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
  location /cors-proxy/ {
    # requests limit
    limit_req zone=one ;#burst=50;
    # set client body size to 2M
    client_max_body_size 2M;
    proxy_pass http://127.0.0.1:3504/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    # Para coger ip de visitante añadir las siguientes dos lineas
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
  location /api/ {
    # requests limit
    limit_req zone=one ;#burst=50;
    proxy_pass http://127.0.0.1:3505/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    # Para coger ip de visitante añadir las siguientes dos lineas
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
  location /sp500/ {
    # requests limit
    limit_req zone=two ;#burst=50;
    proxy_pass http://127.0.0.1:3506/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
    # Para coger ip de visitante añadir las siguientes dos lineas
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name test.domain.com;

  include /etc/nginx/sites-available/domain.conf;

  location /admin {
    # requests limit
    # limit_req zone=one ;#burst=50;
    auth_basic "Area 51";
    auth_basic_user_file /etc/nginx/.admin.pass;
    root /var/www/domain;
    index admin.html;
  }
}

```

---

## WEBMIN

* **Instalacion**

[Descargalo en .deb aqui](http://www.webmin.com/download.html)
`dpkg -i package.deb` 

Si debian protesta de dependencias instala las que pida. Estas son las mas posibles:  
`perl libnet-ssleay-perl openssl libpam-runtime libio-pty-perl python
libauthen-pam-perl libio-pty-perl apt-show-versions`  

* **Modules**

`Modulo para nginx`- [https://www.justindhoffman.com/project/nginx-webmin-module](https://www.justindhoffman.com/project/nginx-webmin-module)  

webmin -> webmin configuration -> webmin modules  

* **Actualizacion**

Despues de actualizarse hay veces que no arranca   
```sh
/etc/init.d/webmin stop
/etc/init.d/webmin start
```

* **Cambiar el password**

`/usr/share/webmin/changepass.pl /etc/webmin root NEWPASSWORD` - Para cambiar la contraseña desde la consola 

* **Usar Lets Encrypt con Webmin**

webmin > webmin Configuration > SSL Encryption > SSL Settings

![Webmin LetsEncrypt](/_img/varios/webmin.png)

le damos a aplicar y reiniciamos el servidor nginx 

---

## LETS ENCRYPT

[HTTPS certbot](https://certbot.eff.org/)

`apt-get install certbot` - para instalarlo    

* **Crear certificado**

```sh
// primero parar nginx
service nginx stop

// Usar la opcion standalone y creamos todos los dominios juntos 
// (dominio.com www.dominio.com sub.dominio.com etc) que luego todo 
// se hace mas facil.
certbot certonly

Saving debug log to /var/log/letsencrypt/letsencrypt.log

How would you like to authenticate with the ACME CA?
-------------------------------------------------------------------------
1: Place files in webroot directory (webroot)
2: Spin up a temporary webserver (standalone)
-------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
Please enter in your domain name(s) (comma and/or space separated)  (Enter
'c' to cancel):domain.com www.domain.com sub.domain.com
Obtaining a new certificate
Performing the following challenges:
tls-sni-01 challenge for domain.com
tls-sni-01 challenge for www.domain.com
Waiting for verification...
Cleaning up challenges
Generating key (2048 bits): /etc/letsencrypt/keys/0067_key-certbot.pem
Creating CSR: /etc/letsencrypt/csr/0067_csr-certbot.pem

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/domain.com/fullchain.pem. Your cert will
   expire on 2017-11-11. To obtain a new or tweaked version of this
   certificate in the future, simply run certbot again. To
   non-interactively renew *all* of your certificates, run "certbot
   renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

* **Borrar certificado**

`certbot delete` - y seleccionamos el que queramos borrar  

* **Renovaciones**

`certbot renew --dry-run` - solo testea    
`certbot renew` - realiza la renovacion  
`certbot renew --force-renew` - fuerza la renovacion aunque falte mucho tiempo aun para su caducidad    

---

## **MACOS**

### PATH

`~/.bash_profile`  
`~/.bashrc`  

### HOMEBREW

[Homebrew](http://brew.sh)  

al Path  

`export PATH=/usr/local/bin:/usr/local/sbin:$PATH`  

* **Install**

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

* **Commands**

`brew doctor` -  
`brew list` -   
`brew install package` - installs `/usr/local/Cellar`  
`brew uninstall package` -   
`brew update` - update brew  
`brew upgrade` - update all packages    
`brew upgrade package` - update package  
`brew outdated` -  
`brew outdated -g` - list packages with updates     
`brew info package` -    
`brew search package` -    
`brew cleanup` -   
`brew cleanup packages` - delete old versions  

---
