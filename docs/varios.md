# MISCELLANEUS

---

## IMAGEMAGICK

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

* Optimizar imagenes

```sh
// png
convert INPUT.png -strip [-resize WxH] [-alpha Remove] OUTPUT.png

//jpg
convert INPUT.jpg -sampling-factor 4:2:0 -strip [-resize WxH] 
[-quality 85] [-interlace JPEG] [-colorspace Gray/sRGB] OUTPUT.jpg
```

---

## GIT

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

* **Commands**

`git init [project-name]` - Añade el directorio donde estamos y crea el .git  
`git status` - lista todos los archivos nuevos o modificados sin haber sido committed    

`git add [file]` - - Añade el fichero a la staging area y empieza a seguir los cambios que en el se producen   
`git add .` -  Añade todo en el directorio de forma recursiva a la staging area    
`git reset [file]` - Saca al archivo de la staging area    

`git diff` - Vemos los cambio del archivo  
`git diff --staged` - Vemos los cambio que hemos mandado al staging  
`git commit -m "mensaje"` - Realiza un commit y le asigna un "mensaje"  

`git log` - Es un diario con los cambios hechos en commits an la actual rama    

* **de .gitignore a github**

Si añadimos algo a .gitignore ya no se commiteara pero tampoco desaparece lo existente en github. Para que se elimine de github ...

`git rm -r --cached some-directory`  
`git commit -m 'Remove the now ignored directory "some-directory"'`  
`git push origin master` - normalmente con `git push` vale

* **ramas**

`git checkout -b nuevaRama` - crea nueva rama de rama nuevaRama  
`git checkout -d nombreRama` - delete rama nombreRama  
`git checkout otraRama` - cambia a la rama otraRama    
`git push origin nombreRama` - push los commits de nombreRama  

`git stash` - cambios no committed se arrastran de una rama a otra. Para mantenerlos separados hacemos un stash antes de movernos a otra rama para guardar esos cambios antes de cambiar de rama.     
`git stash apply` - cuando vuelves a una rama recuperas los cambios sin hacer commit que se guardaron en esa rama.    

---

## MKDOCS


* **Instalacion**

`apt-get install python-pip`   
`pip install mkdocs`   
`pip install -U mkdocs` -  actualizar mkdocs  

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

`mkdocs build -d ruta/donde/gustes` - crea el resultado y con el nombre gustes    

* **Ejemplo mkdocs.yml**

```yml
site_name: Chuletas
google_analytics: ["UA-114956807-1", "auto"]
repo_url: https://github.com/jolav/chuletas
strict: true

nav:
  - 'Menu': 'index.md'
  - 'HTML': 'html.md'  
  - 'CSS': 'css.md'
  - 'Front End': 'frontend.md'
  - 'Javascript': 'javascript.md'
  - 'Javascript snippets': 'javascript-snippets.md'
  - 'Reactjs': 'reactjs.md'
  - 'Vuejs': 'vuejs.md'
  - 'Nodejs con Expressjs': 'nodejs-expressjs.md'
  - 'Nodejs snippets': 'nodejs-snippets.md'
  - 'Nodejs Bases de Datos': 'nodejs-bases-de-datos.md'
  - 'Golang': 'golang.md'
  - 'Golang web': 'golang-web.md'
  - 'Golang snippets': 'golang-snippets.md'
  - 'Golang Bases de Datos': 'golang-bases-de-datos.md'
  - 'Debian': 'debian.md'
  - 'Varios': 'varios.md'

#theme: alabaster
#theme_dir: "chuletas-theme"
theme:
  name: alabaster
  custom_dir: chuletas-theme
site_dir: 'chuletas'
docs_dir: 'docs'
extra_css:
- '_extra/css/extra.css'
extra_javascript:
- '_extra/js/highlight.pack.js'
extra:
  logo: '_img/chuletas128.png'
  include_toc: true
```

---

## GLITCH.COM

Plantilla package.json

```json
{
  "name": "sp500",
  "version": "0.1.11",
  "description": "betazone finance sp500 stock real time data",
  "main": "sp.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node sp.js"
  },
  "engines": {
    "node": "8.x"
  },
  "author": "jolav",
  "license": "BSD-3-Clause",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/jolav/betazone.git"
  },
  "bugs": {
    "url": "https://github.com/jolav/betazone/issues"
  },
  "homepage": "https://github.com/jolav/betazone#readme",
  "dependencies": {
    "express": "^4.16.2",
    "dotenv": "^5.0.0",
  }
}
```

## EXPRESIONES REGULARES

* **Flags**

Se añaden despues de la expresion regular para indicar algo

`/expReg/i` - ignora mayusculas o minusculas  
`/expReg/g` - repetir la busqueda a traves de la cadena entera  

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
`X?` - 0 ó instancias de X, vamos que es opcional  
`X{m}` - exactamente m instancias de X  
`X{m,}` - al menos m instancias de X  
`X{m,n}` - entre m y n instancias(incluidas) de X  


* **Especiales**

`{}[]()^$.|*+?\` y `-` entre corchetes deben ser escapados con contrabarra `\`

### Javascript

En Javascript las expresiones regulares han de estar en una sola linea.  
Los espacios en blanco son significativos  
Delimitadas por barras /  

```js
var expreg = /ab+c/;
var expreg = new RegExp('/ab+c/');
expreg.test(variableATestarConLaexpresionRegular) // da true o false
```

* **Metodos**

`regexp.exec(string)` Si la expresion regular casa con el string devuelve un
array  

* array[0] es la subcadena que casa con la expresion regular
* array[1] el texto capturado por el primer grupo
* array[2] el texto capturado por el segundo grupo
* array[n] el texto capturado por el n grupo
* Si la expresion regular tiene una bandera g la busqueda empieza en la posicion regexp.lastIndex (inicialmente cero). Si la busqueda es exitosa regexp.lastIndex pasara a ser el siguiente caracter tras la coincidencia. Si falla pasa a 0.  
* Esto permite buscar varias coincidencias en una sola cadena

`regexp.test(string)` Si la expresion regular casa con el string devuelve
true, si no false
```js
var b = /&.+;/.test('frank & beans'); // b is true
```  

`string.replace(expreg, nueva)` - en la cadena string reemplaza lo que casa con la expresion regular por la string nueva  

`string.match(regexp)` - busca la regexp en la cadena string y devuelve las coincidencias en un array

```js
var str = "The rain in SPAIN stays mainly in the plain";
var res = str.match(/ain/g); 
console.log(res) // [ 'ain', 'ain', 'ain' ]
```

### ExpReg Comunes

`.replace(/\s+/g, "")` - Elimina todos los espacios en blancos  
`.replace(/\s/g, "loQueSea")` - Reemplaza todos los espacios en blanco por
loQueSea   

`/^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/` - direccion de correo valida

[Aqui](http://code.tutsplus.com/tutorials/8-regular-expressions-you-should-know--net-6149)

---


---