# DEBIAN 

---

## **TESTING**

Avoid problems  
`aptitude install apt-listbugs`

<code>[Debian Package Searcher](https://tracker.debian.org/)</code>

* **Repositories** /etc/apt/sources.list

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
## BackPorts
deb http://ftp.debian.org/debian stretch-backports main
# apt-get -t stretch-backports install "package"

## Dropbox
deb [arch=i386,amd64] http://linux.dropbox.com/debian/ sid main 
## node
deb https://deb.nodesource.com/node_6.x/ stretch main
deb https://deb.nodesource.com/node_8.x/ stretch main

# Chrome
deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main
### Insync
deb http://apt.insynchq.com/debian stretch non-free contrib

## Mysql
deb http://repo.mysql.com/apt/debian/ jessie mysql-apt-config
deb http://repo.mysql.com/apt/debian/ jessie mysql-5.7
deb http://repo.mysql.com/apt/debian/ jessie mysql-tools
deb http://repo.mysql.com/apt/debian/ jessie mysql-tools-preview
```

`apt install aptitude htop smartmontools sshpass rsync curl wget nano apt-transport-https iperf python zip arc arj bzip2 cabextract lzop nomarch p7zip p7zip-full pax tnef unrar-free unzip zoo unrar`

`curl -sSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add -`

---

## **COMMANDS**

* **Add font**

`fc-cache -f -v` - after copying the font to `/usr/share/fonts`  

* **gpg**

`gpg -c file.txt` - ciphers to binary file   
`gpg -ca file.txt` - ciphers to text file  
`gpg --output output.txt -d file.(gpg or txt)` - decipher 

```ssh
// turn directory into a file
tar czf myfiles.tar.gz mydirectory/
// turn file back into a directory
tar xzf myfiles.tar.gz
```

`echo RELOADAGENT | gpg-connect-agent` - removes key from memory. From now on will ask key to decompress

* **zip**

`zip -er zipName.zip path/to/folder/` - Compressing folder with password

* **dpkg**

`dpkg -i package.deb` -   
`apt-get -f install` - repair unfulfilled dependencies if any
`dpkg -r onlyThaName` - uninstall   

* **File and folder size**

`ls -lh`
`du -ah /path`
`du -h -d1`

* **Clean**

```sh
apt-get autoclean &&  apt-get autoremove &&
apt-get remove --purge `deborphan`
```

`dpkg -l | grep -v ^ii | awk '{print $2}' | sed '1,5d'|xargs
 dpkg --purge`

* **Process** 

`pgrep processName` - search    
`ps` - list all processes 
`kill processNumber` -kill process  
`fg` - will continue the process if was stopped  

* **Find**

`find /path -name fileName` - Find file    
`find /path -type d -name dirName` - Find folder 

* **Create user**

`useradd -d /home/username -m -s /bin/bash username` - Creates user with home folder and tty  
`passwd username` -  Ask password for user     

* **Time**

`dpkg-reconfigure tzdata` - Configure timezone  


* **cpulimit**

`apt-get install cpulimit`  
`ps aux | grep nombreProceso` - to get process id  
`cpulimit -p 4510 -l 30`  
`cpulimit -e processName -l 50`  
Add `&` to recover console control  

* **get debian version**

`lsb_release -a`

* **dns**

`dig jolav.me @9.9.9.9`  


* **cron**

```ssh
crontab -e

// activate cron logs
nano /etc/rsyslog.conf
// uncomment 
#cron.*                     -/var/log/cron
service rsyslog restart
```

* **ssh**

Prevent broken pipe disconnect
```ssh
nano /etc/ssh/ssh_config

Host *
ServerAliveInterval 120
```

* **nmap**

```ssh
nmap -Pn X.X.X.X || hostname
```

---

## **SECURITY**

###  SSH

`aptitude install openssh-server openssh-client`  
`nano /etc/ssh/sshd_config`

```sh
// Avoid root login
# Authentication:
LoginGraceTime 120
PermitRootLogin without-password
StrictModes yes

# To enable empty passwords, change to yes (NOT RECOMMENDED)
PermitEmptyPasswords no
```

`service ssh restart`

* **Broken pipe**

```ssh
nano -c /etc/ssh/ssh_config
Host *
ServerAliveInterval 120
```

### CLAMAV

`apt-get install clamav clamav-docs clamav-daemon clamav-freshclam`  

`nano /etc/clamav/freshclam.conf` - 
`service clamav-freshclam restart` - 

```sh
// update as root
freshclam

// if is blocked
/etc/init.d/clamav-freshclam stop 
//then => service clamav-freshclam start

// scan as normal user
clamscan -r /path
```

### RKHUNTER

`aptitude install rkhunter` 

```sh
//update as root
rkhunter --propupd
rkhunter --update

//as root
rkhunter --check
```

### FAIL2BAN

`apt-get install fail2ban whois`  
`cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`  
`nano /etc/fail2ban/jail.local`  

```sh
[DEFAULT]
# "ignoreip" can be an IP address, a CIDR mask or a DNS host
ignoreip = 127.0.0.1 192.168.1.0/24
bantime  = 1800
maxretry = 3
```

```sh
# Destination email address used solely for the interpolations in
# jail.{conf,local} configuration files.
destemail = root@localhost
```

```sh
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
maxretry = 3
```

`service fail2ban restart`  
`service fail2ban status`

ban logs `/var/log/fail2ban.log`  
also `/var/log/auth.log`   

* **manual unban**

`fail2ban-client set ssh unbanip X.X.X.X`  

### UFW

`apt-get install ufw`  

`ufw allow ssh/tcp`  
`ufw logging on`  
Only now we can activate  
`ufw enable`

`ufw allow smtp/tcp`  
`ufw allow http/tcp`  
`ufw allow https/tcp`  
`ufw allow webmin`  

`ufw status` - rules list  
`ufw status numbered` - numbered rules list  
`ufw delete X` - delete rule X   

---

## **PM2**

* **Install**
  
`npm install pm2 -g`  
`npm remove pm2 -g`  

* **Configuration**

[DOCS](http://pm2.keymetrics.io/docs/usage/quick-start/)

`pm2 startup` - as user then follow instructions until pm2 start on reboots as normal user    
`pm2 unstartup` - undo the above

**NOT ALWAYS** To execute the processes as normal user and not as root   
`chmod -R 777 /home/user/.pm2` 

* **Comandos**

```sh
pm2 start app.js  
pm2 kill // stop process, on reboot starts again
pm2 list
pm2 stop all|number
pm2 restart all|number
pm2 delete 7 // delete process number 7
pm2 save // save process list
pm2 start app.js --name "my-name"
```

* **dev**

```sh
pm2-dev start app.js
pm2-dev start app.js --ignore folder1,folder2,file3.txt
```

* **cluster variables**

```ssh
//  -i number-of-workers
pm2 start name.js -i max  
process.env.NODE_APP_INSTANCE;
process.env.name
process.env.pm_id
```

* **max-memory-restart**

```ssh
pm2 start geoip.js -i max --max-memory-restart 1300M

50M
50K
1G
```

* **scale**

```ssh
// Adds 3 new processes to the current ones running
pm2 scale app +3
// Deletes 3 processes from the currently running ones
pm2 scale app 3
```

* **cluster logs in the same file**

```ssh
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'geoip',
    script: 'geoip.js',
    output: './../logs/hits.log',
    error: './../logs/errors.log',
    instances: 2,
    merge_logs: true,
    log_date_format: 'YYYY-MM-DD HH:mm:ss',
  }]
};

pm2 start ecosystem.config.js
```

---

## NPM NON ROOT

How to install packages globally for a given user.

```ssh
// create folder for global packages
mkdir "${HOME}/.npm-packages"

// create .npmrc
nano ${HOME}/.npmrc
prefix=${HOME}/.npm-packages

//edit .bashrc
nano ${HOME}/.bashrc
NPM_PACKAGES="${HOME}/.npm-packages"
PATH="$NPM_PACKAGES/bin:$PATH"
unset MANPATH # delete if already modified MANPATH elsewhere
export MANPATH="$NPM_PACKAGES/share/man:$(manpath)"

source ~/.profile
```

---

## **SYSTEMD**

`systemctl status servidorGO` - gives us service information

* **Create Service**

```sh  
nano /etc/systemd/servidorGO.service  
cp servidorGO.service /etc/systemd/system 
systemcl enable servidorGO.service 
service servidorGO.service start
```

* **Delete service**

```sh
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
systemctl daemon-reload
systemctl reset-failed
```

* **name.service**

```sh
[Unit]
Description= Task description

[Service]
User=user
Group=www-data
Restart=on-failure   
WorkingDirectory=/var/www/path/to/binary/folder
ExecStart=/var/www/path/to/binary/folder/binaryName

[Install]
WantedBy=multi-user.target
```

* **Commands**

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
// Required files
/etc/systemd/servidorGO.service
/etc/systemd/system/servidorGO.service
/etc/systemd/system/multi-user.target.wants/servidorGO.service
/sys/fs/cgroup/systemd/system.slice/servidorGO.service
``` 

* **Another option ?**

```ssh
nano /lib/systemd/system/name.service

[Unit]
Description= Task description

[Service]
Type=simple
Restart=always
RestartSec=5s
ExecStart=/var/www/path/to/binary/folder/binaryName

[Install]
WantedBy=multi-user.target

service name start
service name status

// to enable it on boot
systemctl enable name.service


```

---

## **NGINX**

* **Install**

`apt-get install nginx`  
 
`chown -R user:user /var/www` - Give the user permissions for all that folder  
`mkdir -p /var/www/site1`  
`mkdir -p /var/www/site2`  

### CONFIG

`nano /etc/nginx/nginx.conf`  

Configuration file is called `default` in `/etc/nginx/sites-available`. We delete it or rename it to `default.old`. We must delete the copy in `/etc/nginx/sites-enabled`

Now we can create our own config files  
`nano /etc/nginx/sites-available/domain`    
`cp /etc/nginx/sites-available/domain /etc/nginx/sites-enabled/domain`   
`service nginx restart`


#### server side includes

[Server Side Includes](http://nginx.org/en/docs/http/ngx_http_ssi_module.html)  
[Directives and variables](http://fortboise.org/useful/ssi-manual.html)

`ssi on` - `/etc/nginx/sites-available`

Be careful, paths are from the nginx web server root for that location

```nginx
<!-- Head -->
<!--#include file="/_public/templates/head.html" -->
```

#### hide nginx version

```nginx
nano /etc/nginx/nginx.conf
http {
  server_tokens off;
}
```

#### headers

Be carrful, can restrict the behavior of the applications

```nginx
# Headers to be added:
add_header Strict-Transport-Security "max-age=15768000; 
  includeSubDomains" always;
add_header X-Frame-Options "DENY";
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1; mode=block";
add_header Content-Security-Policy "default-src 'self'";
```

On location block in order to avoid searching robots
```nginx
add_header  X-Robots-Tag "noindex, nofollow, nosnippet, noarchive";
```


#### Limit file upload size

```nginx
location /count-loc/ {
  # set client body size to 10M
  client_max_body_size 10M;
}
```

#### rate Limit

```nginx
/etc/nginx/nginx.conf, here define for example zone=one
# limit requests
limit_req_zone  $binary_remote_addr  zone=one:10m   rate=2r/s;

//then we apply, better on location than server block
/etc/nginx/sites-available/domain
location /count-loc/ {
        # requests limit
        limit_req zone=one burst=20;
}
```

* **Apply limits per IP**

```nginx
geo $limit {
        default 1;
        X.X.X.X 0;
        Y.Y.Y.Y 0;
}
map $limit $limit_key {
        0 "";
        1 $binary_remote_addr;
}
limit_req_zone  $limit_key  zone=one:10m   rate=5r/s;
limit_req_zone  $limit_key  zone=two:10m   rate=1r/m;
limit_req_zone  $limit_key  zone=three:10m   rate=12r/m;
limit_req_zone  $limit_key  zone=four:10m   rate=2r/s;
```

#### http2

```nginx
/etc/nginx/nginx.conf use only very secure ciphers
ssl_ciphers EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:
RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;

// we must generate new keys, do in /etc/nginx/ssl
openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048

cd /etc/nginx/sites-available/domain

// http2 only works over https so only in 443 block
listen 443 ssl http2 default_server;
listen [::]:443 ssl http2 default_server;

# my cipher
ssl_dhparam  /etc/nginx/ssl/dhparam.pem;

// best performance
ssl_session_cache shared:SSL:5m;
ssl_session_timeout 1h;
```

#### cache

```nginx
// own block on sites-availables 
# Expires map
map $sent_http_content_type $expires {
    default                    off;
    text/html                  epoch;
    text/css                   7d;
    application/javascript     7d;
    ~image/                    max;
}

// add in server block when we want to use it
expires $expires;
```

#### private zone

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
mkdir /var/www/error => 404.html 404.css , js, png ...

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

#### block domains

```ssh
map $http_referer $bad_referer {
    default                  0;
    "~spamdomain1.com"       1;
    "~spamdomain2.com"       1;
    "~spamdomain3.com"       1;
}

if ($bad_referer) {
    return 444;
}
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

### PHP subfolder

[Thanks serversforhackers.com](https://serversforhackers.com/c/nginx-php-in-subdirectory)

```nginx
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name domain.com;

  include /etc/nginx/sites-available/domain.conf;

  # enable CORS
  #add_header Access-Control-Allow-Origin '*';


  location / {
          root /var/www/domain/beta;
          index index.html;
  }
  location ^~ /blog {
          alias /var/www/wp-domain;
          index index.php;
          try_files $uri $uri/ @nested;
          location ~ \.php$ {
                  include snippets/fastcgi-php.conf;
                  fastcgi_param SCRIPT_FILENAME $request_filename;
                  fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
          }
  }
  location @nested {
          rewrite /blog/(.*)$ /blog/index.php?/$1 last;
  }
  location ~ /\. {
          deny all;
  }
  location ~* /(?:uploads|files)/.*\.php$ {
          deny all;
  }

}
```

* **Dokuwiki**

```nginx
# DokuWiki
location ^~ /wiki {
        alias /var/www/dokuwiki;
        index index.php;
        try_files $uri $uri/ @nested;
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_param SCRIPT_FILENAME $request_filename;
                fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }
        # dokuwiki security
        location ~ /wiki/(conf|bin|inc)/ {
                deny all;
        }
        location ~ /wiki/data/ {
                internal;
        }
}

location @nested {
        rewrite /wiki/(.*)$ /wiki/index.php?/$1 last;
}
location ~ /\. {
        deny all;
}
location ~* /(?:uploads|files)/.*\.php$ {
        deny all;
}
```

---

## WEBMIN

* **Install**

[Download .deb](http://www.webmin.com/download.html)
`dpkg -i package.deb` 

Possible dependencies fail :   
`perl libnet-ssleay-perl openssl libpam-runtime libio-pty-perl python
libauthen-pam-perl libio-pty-perl apt-show-versions`  

* **Modules**

`nginx module`- [https://www.justindhoffman.com/project/nginx-webmin-module](https://www.justindhoffman.com/project/nginx-webmin-module)  

webmin -> webmin configuration -> webmin modules  

* **Update**

Sometimes doesnt start after update
```sh
/etc/init.d/webmin stop
/etc/init.d/webmin start
```

* **Change password**

`/usr/share/webmin/changepass.pl /etc/webmin root NEWPASSWORD` - Change password from console

* **Lets Encrypt Webmin**

webmin > webmin Configuration > SSL Encryption > SSL Settings

![debian](./_img/debian/webmin-letsencrypt.png)

Apply -> service nginx restart

---

## LETS ENCRYPT

[HTTPS certbot](https://certbot.eff.org/)

`apt-get install certbot` - install    

* **Create certificate**

```sh
// stop nginx
service nginx stop

// Use standalone optionand create all domains together
// (domain.com www.domain.com sub.domain.com etc) 
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

* **List certificates**

`certbot certificates` - List all certificates presents 

* **Add certificate**

`service nginx stop` - stop nginx  
`certbot certonly -d admin.domain.com,api.domain.com,domain.com,sp500.domain.com,www.domain.com,otrosub.domain.com --expand` - 

* **Delete certificate**

`certbot delete` - select the one we want to delete  

* **Renew**

`certbot renew --dry-run` - only test    
`certbot renew` - real renew  
`certbot renew --force-renew` - force renew regardless valid time lasting  
`certbot renew --dry-run --cert-name domain.tld`  
`

* **webroot** , no need of stopping nginx to allow renewals

[read the certot docs](https://certbot.eff.org/#debianstretch-other)

```nginx
server {
  listen 80;
  listen [::]:80;
  server_name beta.jolav.me;
# location / {
#   root /var/www/beta.jolav.me;
#   index index.html;
# }
  return 301 https://beta.jolav.me$request_uri; #
}
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  server_name beta.jolav.me;

  ssl_certificate /etc/letsencrypt/live/beta.jolav.me/fullchain.pem;   #
  ssl_certificate_key /etc/letsencrypt/live/beta.jolav.me/privkey.pem; #
  ssl_session_cache shared:SSL:10m;
  ssl_session_timeout 1h;
  ssl_ciphers HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers on;
  ssl_dhparam  /etc/nginx/ssl/dhparam.pem;

  #add_header  X-Robots-Tag "noindex, nofollow, nosnippet, noarchive";

  location / {
          root /var/www/beta.jolav.me;
          index index.html;
  }
}
```

`certbot certonly --webroot -w /var/www/jolav.me -d jolav.me -d www.jolav.me -d beta.jolav.me`  

* **staging**

`--staging higher rate limits when testing`

---

## VNSTATS

* **vnstat - Bandwidth**

`aptitude install vnstat` - install  

```sh
// list all network interfaces available
vnstat --iflist
// setup interface
vnstat -u -i eth0
// start daemon
/etc/init.d/vnstat start
// vnStat Not Automatically Updating
chown -R vnstat:vnstat /var/lib/vnstat/
```

monitoring (as normal user)

```sh
// realtime
vnstat -l
// historical hour-day-month-week
vnstat -h|d|w|m
// top 10 day
vnstat -t
```

```ssh
crontab -e
0 * * * * node /var/www/vnstat/vnstat.js // execute hourly
vnstat -i eth0 --json d // inside vnstat.js
```

---

## VPS

* **Benchmark**

```ssh
(curl -s wget.racing/nench.sh | bash; curl -s wget.racing/nench.sh | bash)
 2>&1 | tee nench.log
```

```ssh
wget -qO- bench.sh | bash
```

* **Ram**

```
dmidecode -t 17

dd if=/dev/zero of=/dev/null bs=1024M count=200
```

* **iperf**
```ssh
apt install iperf
iperf -c iperf.online.net
```


* **speedtest**

```ssh
curl -Lo speedtest-cli
https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
chmod +x speedtest-cli
```

* **hard disk**
```ssh
apt install smartmontools
// info
smartctl -a /dev/sda 
// test
smartctl -t short|long|select /dev/sda
// result
smartctl -a /dev/sda 
// or only test
smartctl -l selftest /dev/sdc

// info
hdparm -I /dev/sda
// test
hdparm -tT /dev/sda

// test
dd if=/dev/zero of=/tmp/output conv=fdatasync bs=384k count=1k;
 rm -f /tmp/output
```

* **more benchmarks**

```ssh
openssl speed rsa 
openssl speed -multi numCores rsa2048 

7z b
```

* **SYS arm-XT** fix slow upload speeds

[lowendtalk](https://lowendtalk.com/discussion/146114/new-soyoustart-2018-prices/p8)

```ssh
apt install linux-image-4.9.0-6-armmp linux-headers-4.9.0-6-armmp
apt purge *armada*
reboot

modprobe error ufw
wget http://last.public.ovh.hdaas.snap.mirrors.ovh.net/ubuntu/pool
/main/l/linux-modules-armada375/linux-modules-armada375_4.5.2-4_armhf.deb
dpkg -i linux-modules-armada375_4.5.2-4_armhf.deb
```

* **rsync**


**From local to remote**
```ssh
rsync -aP /home/user/data/ user@destiny.com:/home/user/data

# automated without prompt password
rsync -arv --delete -e "sshpass -f '/file/with/passwd' || -p 'password' 
ssh -p PORT -o StrictHostKeyChecking=no" --progress /source/data/
user@domain.com:/destination/data
```

**From remote to local***
```ssh
rsync -charvzP --delete -e "sshpass -p 'pass' ssh -p port" --progress
user@remote.host:/remote/path /local/path/
```

---

## TEST API

* **siege**

```ssh
siege -c 30 -r 1 --no-parser https://api.codetabs.com/v1/
```

---

## OPENVPN

[Easy install](https://github.com/angristan/openvpn-install)

* **Kill Switch**

```ssh
// ON-openvpn.sh

#!/bin/bash
ufw reset
ufw default deny incoming
ufw default deny outgoing
ufw allow out on tun0 from any to any
ufw enable
```

```ssh
// OFF-openvpn.sh

#!/bin/bash
ufw reset
ufw default deny incoming
ufw default allow outgoing
ufw enable
```

`chmod +x ON-openvpn.sh OFF-openvpn.sh`

1. Connect to your openVpn  
2. ./on-openvpn.sh  
3. ./off-openvpn.sh // When you have done  

[more](https://www.nukeador.com/06/07/2017/vpn-kill-switch-for-linux-protect-from-vpn-drops-and-dns-leaks/)

## FLASH FIREFOX

Download from [here](https://get.adobe.com/flashplayer/)

```ssh
tar -xzf flash_player_ppapi_linux*.tar.gz
cp libpepflashplayer.so /usr/lib/mozilla/plugins
cp -r usr/* /usr
// restart firefox
```

---


## MACOS

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

### NGINX

`brew install nginx`  

```ssh
nano /usr/local/etc/nginx/nginx.conf

// nginx will load all files in /usr/local/etc/nginx/servers/

// starts service
nginx 
// stop service
nginx -s stop
```

---

