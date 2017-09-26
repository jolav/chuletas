# MISCELLANEUS

---

## **IMAGEMAGICK**

[Ejemplos](https://www.imagemagick.org/Usage/)

* Hacer el fondo transparente

```sh
convert input.png -transparent red output.png
```

* Conversion entre formatos

```sh
convert image.png image.jpg
// bajando la calidad
convert image.png -quality 90 image.jpg
```

* Convertir color de fondo a transparente o a otro color.  

```sh
convert image.png -background white white.png
```

* Montar imagenes

```sh
montage -label 'FRONT END' frontEnd.png  
 -label 'BACK END' backEnd.png   
 -label 'DATABASE' database.png   
 -label 'UTILS' utils.png   
 -tile 2x2  -geometry 200x200 total.png
```

* Recortar los bordes

```sh
shave
```

* Hacer el canvas mayor para encajar la imagen  

```sh
convert input.png -gravity center -background white -extent 500x500
output.png
```

---

## **GIT**

* **Instalar**

```sh
// Debian backports para pillar la ultima version
deb http://ftp.debian.org/debian strecth-backports main
aptitude -t strecth-backports install "package"

// debian
aptitude install git
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
git config --list
nano ~/.gitconfig
```

* **Local**

`git init` - Añade el directorio donde estamos y crea el .git para llevar  
`git status` -   
`git add fichero.extension` -  Añade el fichero a la staging area y empieza a seguir los cambios que en el se producen  
`git add '*.txt'` -  Podemos usar comodines para añadir cosas. Sin las comillas solo buscara en el directorio actual, pero con las comillas hara una busqueda recursiva  
`git add .` -  Añade todo en el directorio y por debajo a la staging area  
`git add -A .` -  el parametro -A hace que incluso los borrados esten incluidos  
`git commit -m "Comentario que proceda"` -  Hace un commit  
`git log` -  Es un diario con los cambios hechos en commits  
`git rm '*.txt'` -  usa comodin para borrar igual que para sumar  
`git tm -r carpeta` - borra una carpeta y su contenido  
`git stash` -  Guarda tus cambio  
`git stash apply` - Aplica los cambios despues de tu pull  
`git diff HEAD` - Vemos lo cambios de nuestro ultimo commit  
`git diff --staged` - Vemos los cambio que hemos mandado al staging  
`git checkout -- fichero.extension` - Devuelve al fichero a su estado del ultimo commit, -- Indica que no hay mas opciones despues del --  
`git reset fichero.extension` - Saca al archivo de la staging area 

* **GitHub**

Añade la ruta del repositorio remoto en github que usaremos para subir lo que tenemos en local. A git le da igual el nombre del repositorio, se suele usar origin  
`git remote add nombreDelRepo https://github.com/direccion/al/repo`

Sube los commits a nuestro nombreDelRepo en github. La u es para que recuerde parametros y la proxima vez valga con solo git push  
`git push -u nombreDelRepo master`

Nos bajamos el proyecto a local  
`git pull nombreDelRepo master`

Sube los commit una vez ya esta hecho antes y no se necesitan de nuevo los parametros pues ya esta todo configurado  
`git push`

Crea una copia local del repositorio remoto  
`git clone usuario@host:/path/to/repository` ó  
`git clone https://github.com/usuario/repositorio.git /ruta/destino`

* **Ramas(Branches)**

`git branch nuevaRama` - creamos una rama nueva de nombre nuevaRama  
`git branch` - Lista de ramas existentes  
`git checkout nombreOtraRama` - Cambia a esa otra rama  
`git merge nombreOtraRama` - Desde la master mezclamos master con nombreOtraRama  
`git branch -d nombreRama` - Borra la rama de nombre nombreRama  

* **de .gitignore a github**

Si añadimos algo a .gitignore ya no se commiteara pero tampoco desaparece lo existente en github. Para que se elimine de github ...

`git rm -r --cached some-directory`  
`git commit -m 'Remove the now ignored directory "some-directory"'`  
`git push origin master` - normalmente con git push vale

---

## **MKDOCS**


* **Instalacion**

`apt-get install python-pip`  
`pip install mkdocs`  
`pip install -U mkdocs` -  update

instalar mas temas  

`pip install mkdocs-bootstrap`  
`pip install mkdocs-bootswatch`  
`pip install mkdocs-material`  
`pip install mkdocs-alabaster`  
`pip install mkdocs-cinder`  

* **Configuracion**

```sh
mkdocs new my-project
cd my-project
```

```sh
$ mkdocs serve
Running at: http://127.0.0.1:8000/
```

* **Comandos**

`mkdocs build` - crea un directorio site con todo el contenido estatico que se subira al servidor 

`mkdocs build --clean` - limpia lo que sobra de antes  

`mkdocs build -d ruta/done/gustes` - crea el resultado y con el nombre gustes    

* **mkdocs.yml Example**

```yml
site_name: Chuletas
google_analytics: ['UA-65859401-3', 'auto']
repo_url: https://github.com/brusbilis/chuletas
strict: true
extra:
    logo: '_img/logo/brusbb.png'
    #logo_name: false
    include_toc: false
    extra_nav_links:
        #volver a la pagina principal : "https://brusbilis.com"

pages:
- MENU :
    - 'Menu Chuletas': 'index.md'
- CLIENTE:
    - 'html y css': 'cliente/html-css.md'
    - 'javascript': 'cliente/javascript.md'
    - 'javascript para web': 'cliente/javascript-web.md'
    - 'javascript apis': 'cliente/javascript-apis.md'
    - 'jQuery': 'cliente/jquery.md'
    - 'recursos cliente': 'cliente/recursos-cliente.md'
- SERVIDOR:
    - 'node.js': 'servidor/nodejs.md'
    - 'express.js': 'servidor/expressjs.md'
    - 'golang': 'servidor/golang.md' 
    - 'golang para web': 'servidor/golang-web.md'
    - 'bases de datos': 'servidor/bases-datos.md'
    - 'recursos servidor': 'servidor/recursos-servidor.md'
- VARIOS:
    - 'debian y macos': 'varios/debian.md'
    - 'imagemagick, git, mkdocs': 'varios/varios.md'
    - 'expresiones regulares': 'varios/expresiones-regulares.md'

markdown_extensions: 
    - fenced_code

theme_dir: '_chuletas-theme'
#theme: alabaster
site_dir: 'chuletas'
docs_dir: 'docs'
extra_css:
    - '_extra/css/extra.css'
    #- '_extra/css/atom-one-light.css'
    #- '_extra/js/highlight.pack.js'
```

---
