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

---

## FILE

---

## APIs PRIVADAS

---

## WEB SQL DATABASE

---

##

---

##

---

##