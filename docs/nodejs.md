# NODEJS 22.X.X

---

## NPM

`npm install paquete -g` - Instala paquete globalmente.Asi convertimos el paquete un comando ejecutable      
`npm install paquete` - Instala el paquete de forma local. Esto se hace cuando el paquete se necesita para el uso de la app local.     
`npm uninstall paquete -g` - Desinstala paquete global  
`npm uninstall paquete` - Desinstala paquete local  
`npm install package@0.0.0`- Instala una version especifica   

`npm list -g --depth=0` - Lista los paquetes globales instalados  
`npm list --depth=0` - Lista los paquetes locales instalados  

`npm view paquete version` - Muestra la version del paquete  
`npm search package` - Busca un paquete  

`npm update -g` - Actualiza los paquetes globales     
`npm update` - Actualiza los paquetes locales  

`npm outdated -g` - Lista los paquetes globales anticuados    
`npm outdated` - Lista los paquetes locales anticuados  

`npm -v` - Muestra la version de npm que esta instalada  

`npm install -g npm-check-updates` - instala paquete para chequear actualizaciones  
`ncu -g` - chequea paquetes global     
`ncu -g -u` - actualiza los paquetes globales    
`ncu` - chequea paquetes locales  
`ncu -u` - actualiza los paquetes locales de package.json    

`Semver` versiones semanticas

![node](./_img/node/semver.png) 

---

## DOCS

[Documentacion oficial de la API](https://nodejs.org/docs/latest/api/)  

Este enlace va a la ultima version de node, si usas LTS u otra desde ahi te salen todas las opciones  
  

## MODULOS DEL CORE

### fs (sistema de archivos)

### os (sistema operativo)

`const os = require('os')`  

`console.log('Hostname: ' + os.hostname())`  
`console.log('OS type: ' + os.type())`  
`console.log('OS platform: ' + os.platform())`  
`console.log('OS release: ' + os.release())`  
`console.log('OS uptime: ' + (os.uptime() / 60 / 60 / 24).toFixed(1) + ' days')`  
`console.log('CPU architecture: ' + os.arch())`  
`console.log('Number of CPUs: ' + os.cpus().length)`  
`console.log('Total memory: ' + (os.totalmem() / 1e6).toFixed(1) + ' MB')`  
`console.log('Free memory: ' + (os.freemem() / 1e6).toFixed(1) + ' MB')` 

---

## ESModules  

Como de momento con ESM no se puede leer directamente un archivo json  
`import packageJSON from './package.json' with { type: 'json' };`  
hay que hacer lo siguiente

```js
// Sincrona 1
import { createRequire } from "module";
const require = createRequire(import.meta.url);
const data = require("./data.json");
// data ya contiene los datos parseados de data.json

// Sincrona 2
import { readFileSync } from "fs";
const packageJSON = JSON.parse(readFileSync("./package.json"));
```

---

## ASYNC-AWAIT

### Patrones

Funcion asincrona anonima

```js
let main = (async function() {
  const data = await myAsyncFunc();
})();
```

Declaracion de funcion asincrona

```js
async function main() {
  const data = await myAsyncFunc();
};
```

Asignar funcion asincrona

```js
let main = async function() {
  const data = await myAsyncFunc();
};

let main = async () => {
  const data = await myAsyncFunc();
};
```

Objeto y metodo de clase

```js
// Propiedad de objeto
let obj = {
  async method() {
    const data = await myAsyncFunc();
  }
};

// Metodos de clase
class MyClass {
  async myMethod() {
    const data = await myAsyncFunc();
  }
}
```

### Manejo de errores

```js
try {
  const data = await myAsyncFunc();
}
catch(e) {
 // Error!
}
```

---

