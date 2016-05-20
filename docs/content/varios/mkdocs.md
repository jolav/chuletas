# MkDocs

---

### INSTALACION

* Necesitamos python 2.6, 2.7, 3.3, 3.4 o 3.5  
* `apt-get install python-pip`
* `pip install mkdocs`

Para actualizar mkdocs

`pip install -U mkdocs`

---

## CONFIGURACION

### Nuevo Proyecto

```sh
mkdocs new my-project
cd my-project
```

![tools2](/z-static/images/tools/initialLayout.png)

```sh
$ mkdocs serve
Running at: http://127.0.0.1:8000/
```

### Añadiendo Paginas

Añadimos por ejemplo una nueva pagina en `docs/about.md`

Hay que editar `mkdoc.yml` para reflejar el cambio

```sh
site_name: MkLorum
pages:
- Home: index.md
- About: about.md
```

### Elegir Theme

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

### Creando la web

`mkdocs build` crea un directorio `site` con todo el contenido estatico que
se subira al servidor

`mkdocs build --clean` limpia lo que sobra de antes

`mkdocs build -d ruta/donde/gustes` crea el resultado y con el nombre `gustes`

### Ejemplo mkdocs.yml

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




```sh

```
