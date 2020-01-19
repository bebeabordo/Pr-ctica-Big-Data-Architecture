# Pr-ctica-Big-Data-Architecture
## Producto

Recomendador de alojamientos de Airbnb en función de los eventos (Ticketmaster.com)


##  Definición la estrategia del DAaaS:
Generar un informe mensual de los 50 mejores alojamientos de airbnb en función de la calidad y
la cercania de los eventos que se realizaran durante el periodo de estancia.


## Arquitectura DAaaS

La arquitectrura se basará en Google Cloud una vez obtenidas las bases de datos necesarias para
montar el report. Consiste en un crawler que lee los los eventos de feverup.com, extrayendo la 
dirección, la puntuación, la categoría y la fecha en la que se celebrará.
Dicha información volcará un registro por evento en un fichero csv. 

Una vez obtenidos los 2 datasets, se almacenan en Google Storage y mediante HIVE se realiza un 
JOIN para obtener los 50 mejores alojamientos, tomando en cuenta distanicia, puntuación media y 
numero de eventos, siempre con un precio por debajo de 120 euros.

El resultado se almacenará en Google Storage y se enviará a un listado de direcciones de correo
mediante un script de Google Functions.


## Modelo operativo

El crawler se ejectuta de forma manual y local (fuera del entorno cloud) para modificar los
parametros en función de la necesidad del momento.

Tras obtener los datasets se introducirán en Google Storage y a través de Dataproc se levanta
manualmente el cluster para obtener el resumen con HIVE.

Mediante Cloud Functions se ejectuta un Script automáticamente cada vez que se detecta un nuevo
fichero en el directorio, el cual se enviará como adjunto a un listado de direcciones de correo
electrónico.

Una vez ejecutad el proceso se apaga manualmente el cluster.
