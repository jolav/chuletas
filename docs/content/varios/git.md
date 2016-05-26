# GIT

---

## Instalacion

## Conceptos

```sh
Staging area --> lugar donde agrupamos ficheros antes del commit
Commit --> es una instantanea (snapshot) de nuestra repositorio
HEAD --> es un puntero que apunta a tu ultimo commit
```

## Advertencias de ficheros

```sh
staged --> preparados para el commit
unstaged --> tienen cambios pero no has sido preparados para el commit
untracked --> no son seguidos por git (usualmente ficheros recien creados)
deleted --> ha sido borrado y esta esperando a ser removido de git
```

![tools1](/z-static/images/tools/git.jpg)

## Comandos locales

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
`git config --global user.name "John Doe"`
`git config --global user.email johndoe@example.com`

## Comandos GitHub

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

## Ramas(Branches)

`git branch nuevaRama` creamos una rama nueva de nombre nuevaRama  
`git branch` Lista de ramas existentes  
`git checkout nombreOtraRama` Cambia a esa otra rama  
`git merge nombreOtraRama` Desde la master mezclamos master con nombreOtraRama  
`git branch -d nombreRama` Borra la rama de nombre nombreRama  

---
