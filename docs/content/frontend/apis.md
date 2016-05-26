# JAVASCRIPT APIs

Para ver si un navegador tiene una funcionalidad

```js
if (navigator.geolocation) {
  // codigo
} else {
  // codigo
}
```

[Modernizr puede ayudar](https://modernizr.com/)

```js
if (Modernizr.awesomeNewFeature) {
  showOffAwesomeNewFeature();
} else {
  getTheOldLameExperience();
}
```

---

## WEB STORAGE

> Los navegadores guardan sobre 8mb por dominio en un objecto storage    
> Los datos se guardan como propiedades (clave/calor) del objeto storage  
> Acceso a los datos se hace de manera sincrona  
> Los navegadores usan politica del mismo origen, vamos que los datos solo son
> accesibles por otras paginas del mismo dominio  
> Para ello las 4 partes del URL deben coincidir

>>>>>>>   ![apis](/z-static/images/apis/url.png)

>>> 1. Protocolo ¡ ojo que https es distinto a http !  
>>> 2. Subdominio, maps.google.com no puede acceder a datos guardados de
>>> www.google.com  
>>> 3. Dominio debe coincidir  
>>> 4. Puerto, este tambien debe coincidr

### Objeto Storage

* **Propiedades**

`.length` - numero de claves  

* **Metodos**

`.setItem('clave', 'valor')` - Crea una nueva pareja clave/valor  
`.getItem('clave')` - Devuelve el valor de la clave "clave"  
`.removeItem('clave')` - Elimina la clave/valor para esa clave  
`.clear()` - Limpia toda la informacion del objeto storage  

### Session vs Local

>>>>> ![apis](/z-static/images/apis/webStorage1.png)  

* **sessionStorage**

> * Cambios frecuentes cada vez que el usuario visita el sitio, (datos de    localizacion, logueos)  
> * Es personal y privado, no debe ser visto por otros usuarios del dispositivo  

* **localStorage**

> * Solo cambia en los intervalos establecidos (horarios, listas de precios) 
> y es util almacenarlo offline  
> * Cosas que el usuario use de nuevo si vuelve al sitio (guardar preferencias
> y ajustes) 

### IndexedDB

### Web SQL 

Deprecada, pero aun se usa  
No funciona ni en chrome ni en IE  

---

## GEOLOCATION

* **Metodos objeto `navigation.geolocation`**

`getCurrentPosition(exito, error, conf)` -
> exito - funcion para procesar la ubicacion recibida en el objeto `Position` 
> error -funcion para procesar los errores retornados en el objeto 
> `PositionError`  
> conf - objeto para configurar como la informacion sera adquirida  

`watchPosition(exito, error, conf)` - igual que el anterior excepto que 
inicia un proceso de vigilancia para detectar nuevas ubicaciones que nos
enviara cada cierto tiempo  

`clearWatch(id)` - El metodo `watchPosition()` retorna un valor que puede ser
almacenado en una variable para luego ser usado como id aqui y detener la
vigilancia  

* **Propiedades objeto `Position`**  

`.coords.latitude` - Devuelve latitud en grados decimales  
`.coords.longitude` - Devuelve longitud en grados decimales  
`.coords.accuracy` - Precision de latitud y longitud en metros  
`.coords.altitude` - Devuelve metros sobre el nivel del mar  
`.coords.altitudeAccuracy` - Precision de la altitud en metros  
`.coords.heading` - Devuelve grados respecto al norte  
`.coords.speed` - Devuelve velocidad en m/s  
`.coords.timestamp` - Devuelve tiempo desde que se creo (en forma de `Date`)  

* **Propiedades objeto `PositionError`**

`PositionError.code` - Devuelve un numero de error con los valores:  
> 1. Permiso denegado  
> 2. No disponeble  
> 3. Ha expirado el tiempo  

`PositionError.message` - Devuelve un mensaje (pero no para el usuario)  

```js
var elMap = document.getElementById('loc');                 
var msg = 'Sorry, we were unable to get your location.';    

if (Modernizr.geolocation) {                                
  navigator.geolocation.getCurrentPosition(success, fail);  
  elMap.textContent = 'Checking location...';               
} else {                                                    
  elMap.textContent = msg;                                  
}
function success(position) {                                
  msg = '<h3>Longitude:<br>';                               
  msg += position.coords.longitude + '</h3>';               
  msg += '<h3>Latitude:<br>';                               
  msg += position.coords.latitude + '</h3>';                
  elMap.innerHTML = msg;                                    
}
function fail(msg) {                                        
  elMap.textContent = msg;                                  
  console.log(msg.code);                                    
}
```

---

## HISTORY

### Objeto history  

* **Propiedades**

`.length` - numero de articulos en el objeto historia    

* **Metodos**

`history.back()` - Retrocedes en la historia a la anterior posicion  
`history.forward()` - Saltas adelante a la siguiente posicion  
`history.go(n)` - Te lleva a la pagina n respecto de tu posicion que es la 0. 
Por ejemplo -2 echa dos para atras y 1 salta uno hacia adelante   
`history.pushState()` - Añade un elemento a la historia  
`history.replaceState()` - Cambia el actual elemento de la historia por el que 
pasamos ahora  

* **Eventos**

`window.onpopstate` - Usado para manejar los movimientos de usuario de adelante 
atras  

---

## GOOGLE MAPS

### API Key

[Se consigue aqui](https://cloud.google.com/console)

### Ajustes

* **Crear un mapa**

> 1. El evento `onload` llama a la funcion `loadScript()`   
> 2. `LoadScript()` crea un elemento `<script>` que carga la API y cuando se carga
> llama a `init()` que inicializa el mapa  
> 3. `init()`  carga el mapa en la pagina. Primero crea un objeto `mapOptions` 
> con propiedades  
> 4. Luego usa el contructor Map() para crear el map y dibujarlo en la pagina. El 
> contructor tiene dos paremetros  
>     * el elemento dentro del cual el mapa aparecera dentro  
>     * el objeto `mapOption`  

`zoom` - Entre 0 (el mundo entero) y 16  
`mapTypeId` - ROADMAP, SATELLITE, HYBRID, TERRAIN  

    
```js
function init() {
  var mapOptions = {           // Set up the map options
    center: new google.maps.LatLng(40.782710,-73.965310),
    mapTypeId: google.maps.MapTypeId.ROADMAP,
    zoom: 13
  };
  var venueMap;                // Map() draws a map
  venueMap = new google.maps.Map(document.getElementById('map'), 
                                                    mapOptions);
}
function loadScript() {
  var script = document.createElement('script');     
  script.src = 'http://maps.googleapis.com/maps/api/js?sensor=false&
                                                     callback=init';
  document.body.appendChild(script);                 
}
window.onload = loadScript;    
```

* **Cambiar los controles**

>>>>> ![apis](/z-static/images/apis/mapControls.png)

![apis](/z-static/images/apis/controls.png)

```js
function init() {
  var mapOptions = {
    zoom: 14,
    center: new google.maps.LatLng(40.782710,-73.965310),
    mapTypeId: google.maps.MapTypeId.ROADMAP,

    panControl: false,
    zoomControl: true,
    zoomControlOptions: {
      style: google.maps.ZoomControlStyle.SMALL,
      position: google.maps.ControlPosition.TOP_RIGHT
    },
    mapTypeControl: true,
    mapTypeControlOptions: {
      style: google.maps.MapTypeControlStyle.DROPDOWN_MENU,
      position: google.maps.ControlPosition.TOP_LEFT
    },
    scaleControl: true,
    scaleControlOptions: {
      position: google.maps.ControlPosition.TOP_CENTER
    },
    streetViewControl: false,
    overviewMapControl: false
  };
  var venueMap = new google.maps.Map(document.getElementById('map'),
                                                        mapOptions);
}
function loadScript() {
  var script = document.createElement('script');
  script.src = 'http://maps.googleapis.com/maps/api/js?sensor=false
                                                    &callback=init';
  document.body.appendChild(script);
}
window.onload = loadScript;
```

* **Añadir marcadores**

```js
var pinlocation = new google.maps.Latlng(40.782710,-73.965310);
var startPosition = new google.maps.Marker({    // Create marker
  position: pinLocation,                        // Set position
  map: venueMap,                                // Specify the map
  icon: "img/go.png"                            // Path to image 
});
```

---

## CANVAS

### Graficos para la web

`<canvas>` - Crea el lienzo para dibujar  

```html
<body>
  <section id="cajacanvas">
    <canvas id="canvas" width="500" height="300"></canvas>
  </section>
</body>
```

`.getContext(opcion)` - Genera un contexto de dibujo que se asigna al lienzo. 
opcion puede ser "2d" o "webGL"  

```js
function iniciar(){
  var elem = document.getElementById('canvas');
  var canvas = elem.getContext('2d');
}
addEventListener("load", iniciar);
```

### Dibujar 

>>>>> ![apis](/z-static/images/apis/canvasCoords.png)

```js
function iniciar(){
  var elemento=document.getElementById('lienzo');
  lienzo=elemento.getContext('2d');
  // codigo para dibujar y hacer cosas
}
window.addEventListener("load", iniciar, false)
```

* **Rectangulos**

> Metodos  

`fillRect(x,y,ancho,alto)` - Dibuja un rectangulo solido. La esquina superior
izquierda esta en x,y. Ancho y alto definen el tamaño del rectangulo  
`strokeRect(x,y,ancho,alto)` - Como el anterior pero solo dibuja el contorno  
`clearRect(x,y,ancho,alto)` - Es un borrador rectangular  

```js
lienzo.strokeRect(100,100,120,120);
lienzo.fillRect(110,110,100,100);
lienzo.clearRect(120,120,80,80);
```

* **Color**

> Propiedades

`strokeStyle` - color para el contorno de la figura  
`fillStyle` - color para el interior de la figura  
`globalAlpha` - especifica la transfercnia para todas las figuras dibujadas 
en el lienzo  

```js
lienzo.fillStyle = "#000099";
lienzo.strokeStyle = "#990000";
lienzo.strokeStyrl = "rgba(255, 165, 0, 1)"
lienzo.globalAlpha = 0.5                     // (0 opaco, 1 transparente)
```

* **Degradados**

> Metodos

`createLinearGradient(x1,y1,x2,y2)` - Crea un objeto que luego sera usado 
para aplicar un gradiente lineal al lienzo   
`createRadialGradient(x1,y1,r1,x2,y2,r2)` - Crea un objeto que luego será 
usado para aplicar un gradiente circular o radial al lienzo usando dos 
círculos. Los valores representan la posición del centro de cada círculo y 
sus radios  
`addColorStop(posicion,color)` - Posición es un valor entre 0 y 1 que 
determina dónde la degradación comenzará para ese color en particular.
Color especifica los colores que usaran los gradientes  

```js
var gradiente = lienzo.createLinearGradient(0, 0, 10, 100);
gradiente.addColorStop(0.5, '#0000FF');
gradiente.addColorStop(1, '#000000');
lienzo.fillStyle = gradiente;
```

* **Crear Trazados**

Lo normal es procesar figuras en segundo plano y una vez hecho enviarlas al
contexto a ser dibujadas.

Un `trazado` es como un mapa a ser seguido por el lapiz. Puede incluir 
diferentes tipos de líneas, como líneas rectas, arcos, rectángulos ... para
crear figuras complejas  

> Metodos para comenzar y cerrar el trazado 

`beginPath()` - Describe el comienzo de una nueva figura. Se llama primero, 
antes de comenzar a crear el trazado  
`closePath()` - Cierra el trazado generando una linea recta desde el ultimo 
punto hasta el punto de origen. Se puede ignorar cuando usamos el metodo
`fill()` para dibujar el trazado en el lienzo  

> Metodos para dibujar el trazado en el lienzo

`stroke()` - dibuja el trazado de una figura vacia (solo el contorno)  
`fill()` - dibuja el trazado de una figura solida  
`clip()` - declara una nueva area de corte para el contexto. Al inicializar 
el contexto el area de corte es el area completa ocupada por el lienzo. 
`clip()` cambia esa area a una nueva forma creando una mascara. Todo lo que 
este fuera de esa mascara no sera dibujado  

```js
lienzo.beginPath();
// aquí va el trazado
lienzo.stroke();
```

> Metodos para crear el trazado

`moveTo(x,y)` - mueve el lapiz a una posicion para continuar con el trazado  
`lineTo(x,y)` - genera linea recta desde la posicion actual hasta la nueva x,y  
`rect(x,y,ancho,alto)` - genera un texangulo que forma parte del trazado  
`arc(x,y,radio,anguloInicio,anguloFinal,direccion)` - genera un arco o circulo 
en la posicion x,y con radio y desde un anguloInicio hasta anguloFinal. La
direccion false a favor de las agujas del reloj, true en contra  
`quadraticCurve(cpx,cpy,x,y)` - genera una curva cuadratica bezier desde la 
posicion actual hasta las posicion x,y. cpx y cpy indican el punto que dara 
forma a la curva  
`bezierCurve(cp1x,cp1y,cp2x,cp2y,x,y)` - como el anterior pero genera una curva bezier cubica con dos puntos para moldear la curva  

```js
lienzo.beginPath();
lienzo.moveTo(100,100);
lienzo.lineTo(200,200); 
lienzo.lineTo(100,200);
// Opcion 1
lienzo.closePath();     lienzo.stroke();
// Opcion 2
lienzo.fill();
```

```js
// circulos con arc()
lienzo.arc(100,100,50,0,Math.PI*2, false);
// arco de 45 grados
var radianes=Math.PI/180*45;
lienzo.arc(100,100,50,0,radianes, false);
```

* **Estilos de linea**

> Propiedades afectan al trazado completo. para cambia las caracteristicas de
> las lineas hay que crear un nuevo trazado  

`lineWidth` - Determina el grosor de la linea, por defecto = 1  
`lineCap` - Determina la forma de la terminacion de la linea `butt`, 
`round` ó `square`  
`lineJoin` - Forma de la conexion entre dos lineas, `round`, `bevel` 
ó `miter`  
`miterLimit` - Determina cuanto la conexion de dos lineas sera extendida 
cuando lineJoin="miter"   

```js
lienzo.beginPath();
lienzo.arc(200,150,50,0,Math.PI*2, false);
lienzo.stroke();

lienzo.lineWidth=10;
lienzo.lineCap="round";
lienzo.beginPath();
lienzo.moveTo(230,150);
lienzo.arc(200,150,30,0,Math.PI, false);
lienzo.stroke();

lienzo.lineWidth=5;
lienzo.lineJoin="miter";
lienzo.beginPath();
lienzo.moveTo(195,135);
lienzo.lineTo(215,155);
lienzo.lineTo(195,155);
lienzo.stroke();
```

* **Texto**

> Propiedades

`font` - similar a `font` de CSS y acepta los mismos valores  
`textAlign` - Alinea el texto, `start`, `end`, `left`, `right`, y `center`  
`textBaseline` -Alineamiento vertical, `top`, `hanging`, `middle`, 
`alphabetic`, `ideographic`, y `bottom`  

> Metodos

`strokeText(texto,x,y,opcional)` - Dibuja el texto en la posicion x,y como una figura vacia(solo contornos). opcional declara el tamaño maximo,si el texto es 
mas extenso se encogera    
`fillText(texto,x,y)` - Igual que el anterior pero el texto sera solido  
`measureText(texto,x,y)` - Retorna informacion sobre el tamaño de un texto 
especifico. Util para combinar texto con otras formas y calcular posiciones o 
colisiones  

```js
lienzo.font="bold 24px verdana, sans-serif";
lienzo.textAlign="start";
lienzo.fillText("Mi mensaje", 100,100);
```

```js
lienzo.font="bold 24px verdana, sans-serif";
lienzo.textAlign="start";
lienzo.textBaseline="bottom";
lienzo.fillText("Mi mensaje", 100,124);

var tamano=lienzo.measureText("Mi mensaje");
lienzo.strokeRect(100,100,tamano.width,24);
```

* **Sombras**

> Propiedades

`shadowColor` - Color de la sombra usando sintaxis CSS  
`shadowOffsetX` - Recibe un numero que indica cuan lejos esta la sombra del
objeto en direccion horizontal  
`shadowOffsetY` - Recibe un numero que indica cuan lejos esta la sombra del
objeto en direccion vertical  
`shadowBlur` - Produce efecto de difuminacion para la sombra  


```js
lienzo.shadowColor="rgba(0,0,0,0.5)";
lienzo.shadowOffsetX=4;
lienzo.shadowOffsetY=4;
lienzo.shadowBlur=5;
lienzo.font="bold 50px verdana, sans-serif";
lienzo.fillText("Mi mensaje ", 100,100);
```

* **Transformaciones**

`translate(x,y)` - Mueve el origen del lienzo  
`rotate(angulo)` - Rota el lienzo alrededor del origen tantos angulos  
`scale(x,y)` - Incrementa o disminuye las unidades de la grilla para reducir 
o ampliar todo lo dibujado. La escala se puede cambiar solo en un eje. 
Por defecto valor=1  
`transform(m1,m2,m3,m4,dx,dy)` - El lienzo tiene una matriz de valores, esto 
aplica una nueva matriz sobre la actual para modificar el lienzo  
`setTransform(m1,m2,m3,m4,dx,dy)` - Reinicializa la matriz de transformacion 
y establece una nueva con estos valores  

```js
// Moviendo, rotando y escalando.
lienzo.fillText("PRUEBA",50,20);
lienzo.translate(50,70);
lienzo.rotate(Math.PI/180*45);
lienzo.fillText("PRUEBA",0,0);
lienzo.rotate(-Math.PI/180*45);
lienzo.translate(0,100);
lienzo.scale(2,2);
lienzo.fillText("PRUEBA",0,0);
```

```js
// Transformaciones acumulativas sobre la matriz.
lienzo.transform(3,0,0,1,0,0);
lienzo.font="bold 20px verdana, sans-serif";
lienzo.fillText("PRUEBA",20,20);
lienzo.transform(1,0,0,10,0,0);
lienzo.font="bold 20px verdana, sans-serif";
lienzo.fillText("PRUEBA",100,20);
```

* **Restaurar el estado**

> Metodos

`save()` - graba es estado del lienzo  
`restore()` - recupera el ultimo estado grabado  

```js
// Grabando el estado del lienzo.
lienzo.save();
lienzo.translate(50,70);
lienzo.font="bold 20px verdana, sans-serif";
lienzo.fillText("PRUEBA1",0,30);
lienzo.restore();
lienzo.fillText("PRUEBA2",0,30);
```

* **globalCompositeOperation**

> Determina como una figura es poscionada y combinada con figuras ya dibujadas 
> en el lienzo  
> Valores de la propiedad  

`source-over` - POR DEFECTO - la nueva figura sera dibujada sobre las 
existentes  
`source-in` - Solo la parte de la nueva figura que se sobrepone a las figuras previas es dibujada. El resto de la figura, e incluso el resto de las figuras previas, se vuelven transparentes.  
`source-out` - Solo la parte de la nueva figura que no se sobrepone a las 
figuras previas es dibujada. El resto de la figura, e incluso el resto de las figuras previas, se vuelven transparentes.  
`source-atop` - Solo la parte de la nueva figura que se superpone con las 
figuras previas es dibujada. Las figuras previas son preservadas, pero el
resto de la nueva figura se vuelve transparente.  
`lighter` - Ambas figuras son dibujadas (nueva y vieja), pero el color de las partes que se superponen es obtenido adicionando los valores de los colores de cada figura.  
`xor` - Ambas figuras son dibujadas (nueva y vieja), pero las partes que se superponen se vuelven transparentes.  
`destination-over` - Este es el opuesto del valor por defecto. Las nuevas 
figuras son dibujadas detrás de las viejas que ya se encuentran en el lienzo.  
`destination-in` - Las partes de las figuras existentes en el lienzo que se superponen con la nueva figura son preservadas. El resto, incluyendo la nueva figura, se vuelven transparentes  
`destination-out` - Las partes de las figuras existentes en el lienzo que no
se superponen con la nueva figura son preservadas. El resto, incluyendo la 
nueva figura, se vuelven transparentes.  
`destination-atop` - Las figuras existentes y la nueva son preservadas solo en la parte en la que se superponen.  
`darker` - Ambas figuras son dibujadas, pero el color de las partes que se superponen es determinado substrayendo los valores de los colores de cada
figura.  
`copy` - Solo la nueva figura es dibujada. Las ya existentes se vuelven transparentes.  

```js
// robando la propiedad globalCompositeOperation
lienzo.fillStyle="#990000";
lienzo.fillRect(100,100,300,100);
lienzo.globalCompositeOperation="destination-atop";
lienzo.fillStyle="#AAAAFF";
lienzo.font="bold 80px verdana, sans-serif";
lienzo.textAlign="center";
lienzo.textBaseline="middle";
lienzo.fillText("PRUEBA",250,110);
```

### Procesar imagenes

* **drawImage()**

* **Datos de imagen**

* **cross-origin**

* **Extraccion de datos**

* **Patrones**


### Animaciones

* **Elementales**

* **Profesionales**


### Procesar Video

* **Mostrar video**

* **Aplicacion real**

---

## FILE

![apis](/z-static/images/comingSoon2.jpg)

---

## POUCH DB

[PouchDB](https://pouchdb.com/)

---

## REDIS

[Redis](http://redis.io/)

---