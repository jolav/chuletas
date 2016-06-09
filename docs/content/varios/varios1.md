# VARIOS

---

## GIT

### Instalacion

```sh
// repo para Ubuntu
sudo add-apt-repository ppa:git-core/ppa

// para ultima version Debian backports
deb http://ftp.debian.org/debian jessie-backports main
aptitude -t jessie-backports install "package"

// lo normal en debian
aptitude install git
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
git config --list
nano ~/.gitconfig
```


### Conceptos

```sh
Staging area --> lugar donde agrupamos ficheros antes del commit
Commit --> es una instantanea (snapshot) de nuestra repositorio
HEAD --> es un puntero que apunta a tu ultimo commit
```

### Advertencias de ficheros

```sh
staged --> preparados para el commit
unstaged --> tienen cambios pero no has sido preparados para el commit
untracked --> no son seguidos por git (usualmente ficheros recien creados)
deleted --> ha sido borrado y esta esperando a ser removido de git
```

![tools1](/z-static/images/tools/git.jpg)

### Comandos locales

`git init` Añade el directorio donde estamos y crea el .git para llevar  
`git status`  
`git add fichero.extension` Añade el fichero a la staging area y empieza a
seguir los cambios que en el se producen  
`git add '*.txt'` Podemos usar comodines para añadir cosas. Sin las comillas
solo buscara en el directorio actual, pero con las comillas hara una busqueda recursiva  
`git add .` Añade todo en el directorio y por debajo a la staging area  
`git add -A .` el parametro -A hace que incluso los borrados esten incluidos  
`git commit -m "Comentario que proceda"` Hace un commit  
`git log` Es un diario con los cambios hechos en commits  
`git rm '*.txt'` usa comodin para borrar igual que para sumar  
`git tm -r carpeta` borra una carpeta y su contenido  
`git stash` Guarda tus cambio  
`git stash apply` Aplica los cambios despues de tu pull  
`git diff HEAD` Vemos lo cambios de nuestro ultimo commit  
`git diff --staged` Vemos los cambio que hemos mandado al staging  
`git checkout -- fichero.extension` Devuelve al fichero a su estado del ultimo
 commit  
`--` Indica que no hay mas opciones despues del --  
`git reset fichero.extension` Saca al archivo de la staging area  

### Comandos GitHub

Añade la ruta del repositorio remoto en github que usaremos para subir lo que
tenemos en local. A git le da igual el nombre del repositorio, se suele usar
origin
> `git remote add nombreDelRepo https://github.com/direccion/al/repo`

Sube los commits a nuestro nombreDelRepo en github. La u es para que recuerde parametros y la proxima vez valga con solo git push
> `git push -u nombreDelRepo master`

Nos bajamos el proyecto a local
> `git pull nombreDelRepo master`

Sube los commit una vez ya esta hecho antes y no se necesitan de nuevo los
 parametros pues ya esta todo configurado
> `git push`

Crea una copia local del repositorio remoto
> `git clone usuario@host:/path/to/repository` ó  
> `git clone https://github.com/usuario/repositorio.git /ruta/destino`

### Ramas(Branches)

`git branch nuevaRama` creamos una rama nueva de nombre nuevaRama  
`git branch` Lista de ramas existentes  
`git checkout nombreOtraRama` Cambia a esa otra rama  
`git merge nombreOtraRama` Desde la master mezclamos master con nombreOtraRama  
`git branch -d nombreRama` Borra la rama de nombre nombreRama  

---

## EXPRESIONES REGULARES

* **Basicos**

`.` - cualquier cosa  
`\s` - espacio en blanco  
`\S` - cualquiera menos espacio en blanco  
`\d` - numero  
`\D` - no numero  
`\w` - caracter alfanumerico (a-z, A-Z, 0-9)  
`\W` - no caracter alfanumerico  
`\t` - tabulador  
`\r` - retorno de carro   
`\n` - nueva linea   

* **Limites**

`^` - Comienzo de cadena o de multilinea  
`\A` - Comienzo de cadena  
`$` - Fin de cadena o fin de multilinea   
`\Z` - Fin de cadena  
`\b` - Palabra limite , "\bcat\b" pilla en "cat in the hat" pero no en "locate"  
`\B` - No es una palabra limite  
`\<` - Comienzo de palabra   
`\>` - Fin de palabra  

* **Grupos y rangos**

Los rangos son inclusive  

`(X|Y)` - X o Y, ejemplo \b(cat|dog)s\b  pilla "cats" o "dogs"  
`(...)` - Grupos  
`[abc]` - Rango ("a" ó "b" ó "c")  
`[^abc]` - No ("a" ó "b" ó "c")  
`[a-h]` - Minuscula entre la "a" y la "h"  
`[A-H]` - Mayucula entre la "a" y la "h"  
`[3-6]` - Numero entre 3 y 6 ambos inclusive  

* **Cuantificadores**

Por defecto los cuantificadores solo se aplican a un caracter, para aplicarlo 
a mas usar (...) para especificar la cadena buscada  

`X*` - 0 ó mas repeticiones de X  
`X+` - 1 ó mas repeticiones de X  
`X?` - 0 ó instancias de X  
`X{m}` - exactamente m instancias de X  
`X{m,}` - al menos m instancias de X  
`X{m,n}` - entre m y n instancias(incluidas) de X  
`g` - repetir la busqueda a traves de la cadena entera  


* **Especiales**

`{}[]()^$.|*+?\` y `-` entre corchetes deben ser escapados con contrabarra `\`

---

### Expresiones Comunes

`.replace(/\s+/, "")` - Elimina todos los espacios en blancos  
`.replace(/\s/g, "loQueSea")` - Reemplaza todos los espacios en blanco por 
loQueSea   


[Aqui](http://code.tutsplus.com/tutorials/8-regular-expressions-you-should-know--net-6149)

---

## MKDOCS


### Instalacion

* Necesitamos python 2.6, 2.7, 3.3, 3.4 o 3.5  
* `apt-get install python-pip`
* `pip install mkdocs`

Para actualizar mkdocs

`pip install -U mkdocs`

### Configuracion

* **Nuevo Proyecto**

```sh
mkdocs new my-project
cd my-project
```

![tools2](/z-static/images/tools/initialLayout.png)

```sh
$ mkdocs serve
Running at: http://127.0.0.1:8000/
```

* **Añadiendo Paginas**

Añadimos por ejemplo una nueva pagina en `docs/about.md`

Hay que editar `mkdoc.yml` para reflejar el cambio

```sh
site_name: MkLorum
pages:
- Home: index.md
- About: about.md
```

* **Elegir Theme**

Añadir theme: temaElegido al archivo `mkdoc.yml`

```sh
site_name: MkLorum
pages:
- Home: index.md
- About: about.md
theme: readthedocs
```

Añadir mas temas:

> `pip install mkdocs-bootstrap`  
> `pip install mkdocs-bootswatch`  
> `pip install mkdocs-material`  
> `pip install mkdocs-alabaster`  
> `pip install mkdocs-cinder`  

* **Creando la web**

`mkdocs build` crea un directorio `site` con todo el contenido estatico que
se subira al servidor

`mkdocs build --clean` limpia lo que sobra de antes

`mkdocs build -d ruta/donde/gustes` crea el resultado y con el nombre `gustes`

* **Ejemplo mkdocs.yml**

```yml
site_name: Chuletas

pages:
- Home: 'index.md'
- Front-end:
    - 'Html5': 'frontend/html5.md'
    - 'CSS3': 'frontend/css3.md'
    - 'Javascript': 'frontend/js.md'
    - 'jQuery': 'frontend/jquery.md'
- Back-End:
    - 'Golang': 'backend/golang.md'
- Debian:
    - 'Desktop': 'debian/desktop.md'
    - 'Server': 'debian/server.md'
- Varios:
    - 'Git': 'varios/git.md'
    - 'Hugo': 'varios/hugo.md'
    - 'mkDocs': 'varios/mkdocs.md'
    - 'Expresiones regulares': 'varios/expreregulares.md'

markdown_extensions:
    - fenced_code

theme_dir: 'brus-docs-theme'
```

---

## HUGO

### Instalacion

Esta en los repos de debian testing  

`aptitude install hugo`  

###
---






