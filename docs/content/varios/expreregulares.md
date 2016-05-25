# EXPRESIONES REGULARES

---

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


* **Especiales**

`{}[]()^$.|*+?\` y `-` entre corchetes deben ser escapados con contrabarra `\`

---

## EXPRESIONES COMUNES

[Aqui](http://code.tutsplus.com/tutorials/8-regular-expressions-you-should-know--net-6149)

---







---




