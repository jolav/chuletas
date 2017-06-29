# MACOS EL CAPITAN

---

## PATH

Para que el sistema los lea los pongo en los archivos:  

`~/.bash_profile`  
`~/.bashrc`   

---

## NODE.JS

[Bajar el paquete aqui](https://nodejs.org/en/download/)  

### npm como usuario

Para poder instalar paquetes globales de npm como usuario normal podemos elegir:   

* **Cambiar permisos a la directorio de npm**  

`npm config get prefix` - ok si es /usr/local, no hacerlo sobre solo /usr que la 
preparamos  
`sudo chown -R usuario nuevoDirectorio/{lib/node_modules,bin,share}` - esto hay que
hacerlo con un usuario admin para poder usar sudo  

* **Canbiar el directorio por defecto de npm a otro sitio**

` mkdir ~/.npm-global` -   
` npm config set prefix '~/.npm-global'` -  
` export PATH=~/.npm-global/bin:$PATH` - Añadir esta linea a `.bashr` y 
`.bash_profile`   
` source ~/.bashrc` - ` source ~/.bash_profile`    

* **Usar [Homebrew](#homebrew)**

---

## HOMEBREW

[Homebrew](http://brew.sh)  

al Path  

`export PATH=/usr/local/bin:/usr/local/sbin:$PATH`  

### Instalar

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

### Comandos

`brew doctor` -  
`brew list` - lista todos los paquetes instalados  
`brew install paquete` - instala ese paquete en `/usr/local/Cellar`  
`brew uninstall paquete` - desintala el paquete  
`brew update` - actualiza brew  
`brew upgrade` - actualiza todos los paquetes  
`brew upgrade paquete` - actualiza el paquete  
`brew ourdated -g` - lista los paquetes con actualizaciones pendientes   
`brew info paquete` - nos da informacion del paquete  
`brew search paquete` - busca un paquete  
`brew cleanup` -   
`brew cleanup paquetes` - elimina las versiones viejas  
`brew outdated` - lista lo que tiene actualizacion de lo instalado  



---















---



