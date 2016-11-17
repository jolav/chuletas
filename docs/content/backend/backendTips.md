# TRUCOS PARA BACKEND

---

## APIS

### Formato de los datos

`Dar formato a los datos` - para devolver los datos de la API en un formato que sea legible  
```js
res.send('<pre>' + JSON.stringify(result, null, 2) + '</pre>');

// en el frontend getAjax antes de parsear eliminar los pre
callback(JSON.parse(xhr.responseText.slice(5).slice(0, -6)));
```

---