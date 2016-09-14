# DEBIAN TESTING

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

## IMAGE MAGIC

[Ejemplos](https://www.imagemagick.org/Usage/)

Convertir imagen el fondo en transparente

```sh
convert input.png -transparent red output.png
```

Convertir entre formatos de imagen

```sh
convert imagen.png imagen.jpg
// o rebajando compresion
convert imagen.png -quality 90 imagen.jpg
```

Convierte el color transparente o lo que sea de fondo de la imagen por white (o color que se ponga). EL -alpha es para reducir peso de la imagen

```sh
convert image.png -background white -alpha remove white.png
```

Montar imagenes, ejemplo

```sh
montage -label 'FRONT END' frontEnd.png -label 'BACK END' backEnd.png -label 'DATABASE' database.png -label 'UTILS' utils.png -tile 2x2  -geometry 200x200 total.png
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
```



---
