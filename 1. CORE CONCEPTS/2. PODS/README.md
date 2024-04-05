# PODs

Unidad lógica mínima en Kubernetes.

Kubernetes contiene un número de abstracciones que representan el estado de nuestro sistema: desplegar aplicaciones y cargas de trabajo en contenedores,  sus recursos de red y disco asociados, y otra información relativa al clúster. Estas abstracciones se representan con objetos en la API de Kubernetes.

Un **pod** es la unidad básica, la más pequeña y simple en el modelo de objetos de Kubernetes que creamos o desplegamos. Un pod representa un proceso en ejecución en nuestro clúster.

Un pod encapsula un contenedor de aplicación (o múltiples contenedores), recursos de almacenamiento, una **única dirección IP**, y opciones que gobiernan cómo los contenedores deben ejecutarse. Un pod representa una unidad de desplegue: una única instancia de una aplicación en Kubernetes, que puede consistir tanto en un único contenedor como en un pequeño número de contenedores que están estrechamente acoplados y comparten recursos.

Los pods se pueden usar de dos formas:

* **Pods que se ejecutan en un único contenedor**: es el uso más común en Kubernetes, lo **normal**. En este modelo, se puede concebir un pod como un wrapper sobre un único contenedor, y Kubernetes administra el pod en lugar del contenedor directamente.

* **Pods que ejecutan múltiples contenedores y necesitan trabajar juntos**: un pod puede encapsular una aplicación compuesta por contenedores ubicados en la misma localización que están estrechamente acoplados y necesitan compartir recursos. Estos contenedores pueden formar una unidad conjunta donde un contenedor sirve archivos desde un volúmen compartido al público, mientras otro contenedor actualiza o refresca esos archivos. El pod enrrolla estos contenedors y almacena recursos juntos como una única unidad manejable. 


Cada Pod es capaz de ejecutar una isntancia de una aplicación dada. Si queremos escalar nuestra aplicación horizontalmente (ej. con múltiples instancias), debemos usar múltiples pods, uno por instancia. 

Los pods replicados son normalmente creados y administrados como un grupo por una abstracción llamada **Controlador**, por ejemplo el **ReplicaSet** controller que mantiene el ciclo de vida del pod. Esto incluye la creación, actualización, escalación y eliminación del pod.

Todos los contenedores de un pod tienen la misma dirección de red, así que comparten IP y puerto. 
Todos los contenedores de un pod tienen también la misma interfaz de red loopback, por lo que un contenedor puede comunciarse con otros contenedores en el mismo pod mediante localhost.

Un pod tiene una única IP para todos los recursos. 
Los pods se desplegan dentro del nodo de un clúster. Están los nodos del control-plane y los nodos workers. 


## Pods con varios contenedores

Por lo general únicamente habrá un contenedor dentro de un pod ya que es básicamente romper los esquemas lógicos, ya que todos los contenedores tendrán la misma IP, se realizarán backups simultaneas en dichos contenedores, distintos ciclos de vida entre los contenedores que no van a poder ser divisibles, etc. . Si tenemos en un mismo pod los contenedores de Apache, Middleware y backend, resutaría ridículo parar alguno de los servicios como SQL, que puede estar suministrando datos a otros servicios, para actualizar el frontal. 
En cambio, cuando tienes un componente compuesto por módulos intrínsicamente unidos entre sí, puede ser interesante. 

#### TIPOS DE CONTENEDORES EN UN POD COMPARTIDO 

¿Conviene meter varios contenedores en un pod? Como ya hemos recalcado, generalmente no, a no ser que sea necesario, como por ejemplo los Init Container y los Sidecar Container. 

* **Init container**: contenedores que se inician antes que el contenedor principal hasta que se da cierta condición y una vez que se da el init container termina. Se ejecutan en orden secuencial si tenemos varios. Por lo general comprueban si un determinado servicio responde antes de arrancar el contendor principal.
* **Sidecar container**: contenedor que se añade a un pod para realizar una acción secundaria. Un ejemplo típico es un login. Sirven si queremos que los logs de inicio se redirijan a un fichero de texto, ya que los login de docker no lo recopilan. Como los pods comparten almacenamiento, los sidecar recopilan los datos del login y lo almacenan. 
