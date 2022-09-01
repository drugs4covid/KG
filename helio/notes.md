Helio + GraphDB para KG de Drugs4Covid

https://lot.linkeddata.es/#stories

https://oeg-upm.github.io/helio/
publisher

using a triple store with a SPARQL endpoint as 


Java 8

java -jar 

GraphDB
1) crear repositorio 'd4c'
2) cambiar en confuguración 'discovery' por d4c
3) Setup > Repositorioes > Create new > Free Repositorioes > Cambiar nombre/ID
4) Activar repositorio
5) Explore > Graphs overview
6) Import > RDF > Upload Files
(reset implica borrar helio-storage)

localhost:8080/sparql

Explorar Sparql-Endpoint
sección secreta:
helio-api (root/root)
Views > Templating (definir una regex sobre la URI para asociarla al dominio y así Helio renderiza el html)

Para explorar recursos, poner como URI el namespace de kg.drugs4covid.oeg.fi.upm.es para que Helio renderice el recurso

Customizar vista html del recurso: 

templating>
 ^namespace$
crear carpeta 'views' y añadir el archivo 'main.html', con syntaxis thymeleaf


## nueva version

localhost:4201
localhost:4567/api