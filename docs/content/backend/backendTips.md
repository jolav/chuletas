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

## Testing

### Apache Bench

Curiosamente en debian no viene instalado pero si en MacOS.

`apt-get install apache2-utils` -  

-n numero de peticiones  
-c concurrencias

[Mas info](https://httpd.apache.org/docs/current/programs/ab.html)  

```sh
ab -n 1000 -c 10 https://brusbilis.com/
```

---

## EJS

### Pasar variables

Pasar variables de servidor a javascript a traves de ejs  

```ejs
<% if (active) { %>
  <script>
    var active = <%- JSON.stringify(active) %>;  
  </script>
<% } %>

<% if (user) { %>
  <script>
    var user = <%- JSON.stringify(user) %>;
  </script>
<% } else { %>
   <script>
    var user;
  </script>
<% } %>
```

o Usar AJAX  


---
