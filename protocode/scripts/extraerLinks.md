# Extraer links de una página web

Hay un buen número de herramientas para extraer los enlaces de una *página web*, sin embargo a veces resulta más rápido hacerlo semi-manualmente sin salir del navegador, usando la consola (**`F12`**) con una pizca de **`JavaScript`**.

Aquí un ejemplo para extraer los enlaces externos de cualquier página de Wikipedia. Se utiliza el `método` [**`Document.querySelectorAll()`**](https://developer.mozilla.org/es/docs/Web/API/Document/querySelectorAll) para seleccionar todos los enlaces externos que aparecen la página y genera una tabla con ellos.

```JavaScript
var x = document.querySelectorAll("a.external.text");
var myarray = [];
for (var i=0; i<x.length; i++){
var nametext = x[i].textContent;
var cleantext = nametext.replace(/\s+/g, ' ').trim();
var cleanlink = x[i].href;
myarray.push([cleantext,cleanlink]);
};
function make_table() {
    var table = '<table><thead><th>Name</th><th>Links</th></thead><tbody>';
   for (var i=0; i<myarray.length; i++) {
            table += '<tr><td>'+ myarray[i][0] + '</td><td>'+myarray[i][1]+'</td></tr>';
    };
 
    var w = window.open("");
w.document.write(table); 
}
make_table()
```

El resultado no es perfecto, hay falsos positivos, problemas de formato, etc... pero en unos pocos minutos tienes capturados más de 40 enlaces, muchos de ellos relevantes, que son fácilmente convertibles a un fichero **`.csv`** (ver el resultado en [wikiOSINT.csv
](https://github.com/Eclectikus/buscosint/blob/main/scripts/wikiOSINT.csv)).

:information_source: Puedes encontrar más información sobre este método en [este enlace](https://towardsdatascience.com/quickly-extract-all-links-from-a-web-page-using-javascript-and-the-browser-console-49bb6f48127b)