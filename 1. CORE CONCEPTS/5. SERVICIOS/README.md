
# SERVICIOS

Es la forma de conectarnos de forma externa a nuestros contenedores.
Hay clientes que pueden necesitar acceder a los pods de un deployment, que cada uno tendrá su IP, pero, no podremos conectarnos directamente con ellos porque no hay IP fija, no hay nombre fijo y no hay un puerto fijo. 
Si se cae un pod, el replicaSet creará otro pod con una nueva IP.

El servicio será el intermediario, que añade una IP fija, un nombre fijo y un puerto fijo. El servicio hace una lista dinámica de todos los pods y se los ofrece al cliente. 

Tipos de servicios:

- ClusterIP: hace que los pods sean accesibles desde dentro del clúster. 
- NodePort: accesible desde fuera del clúster, con puerto con el que se puede acceder de forma externa llamado nodeport. 
- LoadBalancer: implementa características de ambos, pero que se suele integrar con loadbalancers de terceros, como AWS, GCP, Azure, etc. 

El servicio detecta los distintos pods que se van creando mediante el uso de labels, selectors. 

Objeto que provee Kubernetes para exponer aplicaciones, a través de este podemos describir el tipo de conexiones que se realizan a nuestra aplicación y también balancear la carga destinada a esta. 

El conjunto de pods a los que apunta el servicio se determina por un **selector**, que no es más que un campo que se configura en el propio YAML del servicio y una label que se configura en el YAML del pod. De esta forma, todos los pods con una etiqueta igual a Selector, ejecutará el servicio. 


#### ClusterIP
Expone el Servicio en una dirección IP interna del clúster. Este es el ServiceType por defecto. 
Nos permite exponer nuestro deployment de manera INTERNA, de una forma balanceada. Sin el clusterIP hay conectividad entre pods, pero la carga no está balanceada. Desde la red no podemos llegar a esta aplicación. 

![[DEVOPS/K8S/anexo/Pasted image 20230314153203.png]]

Ejemplo de YAML del servicio:

```
apiVersion: v1.0
kind: Service
metadata:
	name: clusterip-svc
	namespace: service
	labels:
		app: my-app-clusterip
	spec:
		selector:
			app: my-app-clustering
		ports:
			-port: 80
			 targetPort: 80
```

Es importante acordarse que la etiqueta de los pods que queremos que ejecuten estos servicios debe tener una tag igual que el Selector. 

Es el servicio por defecto, por lo que no hace falta poner la etiqueta type en spec. 


#### NodePort (no lo utilizaremos mucho)

Expone un Service en cada IP del nodo en un puerto estático (el NodePort). Automáticamente, se crea un Service ClusterIP, al cual enruta el NodePort del Service. Podrás alcanzar el Service NodePort desde fuera del clúster, haciendo una petición a \<NodeIP\>:\<NodePort\>. El rango de puertos que utiliza este servicio es 30000-32768. 

![[DEVOPS/K8S/anexo/Pasted image 20230314153712.png]]

Cuando alguien envía una petición con la IP del clúster y con el puerto de los nodos externos, el balanceador sabrá que existe este servicio. Desde el balanceador al nodo se hace a través del puerto 32668, pero dentro del clúster el SVC (clúster IP) actuaría como un clústerIP. 
El endpoint se crea cuando se crea un servicio con backend se creará un endpoint y cuáles son los pods que están asociados.
En producción no se trabaja con NodePort, ya que el hecho de que los nodos con la IP pública no es lo correcto.

Hay que poner la etiqueta ``type: NodePort`` en la etiqueta spec. 

Aunque la petición llegue a otro nodo, todos los nodos conocen este servicio, por lo que se reenvía al nodo correcto.


#### LoadBalancer

Expone el service externamente usando el balanceador de carga del proveedor de la nube. 
![[DEVOPS/K8S/anexo/Pasted image 20230314154959.png]]


Este tipo de servicios están pensado para trabajar con la nube. En azure nos crearía una IP pública y se lo asignaría a nuestro balanceador. El backend del balanceador son instancias. El load balancer envía la petición a los nodos del clúster, y son los nodos del clúster quienes envían la petición al servicio (svc). Se accede por la IP pública del balanceador, que enviaría la petición a cualquiera de los nodos del clúster (el que considere conveniente por carga), y los nodos del clúster que son los que si que conocen los servicios se lo enviará a estos servicios, que por último se lo enviarían a los pods. 

Aunque la petición llegue a otro nodo, todos los nodos conocen este servicio, por lo que se reenvía al nodo correcto.


### CREAR SERVICIOS

Existe un comando que sirve para crear servicios:

``kubectl expose <x>``, donde x es un deployment, replicaset, etc. 
Y añadir los siguientes parametros: 

- --name: nombre del servicio.
- --port: puerto en el que escucha el servicio.
- --target-port: puerto por el cual el servicio reenvía las peticiones al backend.
- --type: tipo de servicio, sino se indica es clusterIP.
- --protocol: tipo de protocolo, sino se indica se utilizará TCP. 

Uno de los parámetros del kubectl run es expose, por lo que se puede hacer con un solo comando. 

``kubectl run pod-clusterIP --image=nginx --expose --port=80 -n ddoradog --dry-run=client -oyaml

Este comando nos mostrará un servicio, que será un clusterIP y un pod, cuyo tag es igual al selector del service, por lo que es un nodo enlazado con el servicio.

De forma que si desde otro pod ejecutamos un curl sobre el servicio anterior, nos debería dar respuesta. 

``kubectl run test-clusterip --image=ccurlimages/curl --rm -it --restart=Never -n ddoradog -- curl pod-clusterip.ddoradog:80


---

Los pods de Kubernetes son mortales. Nacen y cuando mueren, no resucitan. Los ReplicaSets en particular crean y destruyen Pods dinámicamente. Si bien cada Pods tiene su propia dirección IP, incluso no se puede confiar que esa dirección IP sea estable a lo largo del tiempo. Esto conlleva a un problema: si algunos grupos de Pods (llamemosles backends) porporcionan funcionalidad a otros pods (llamemosles frontends) dentro del cluster Kubernetes, ¿cómo los frontends encuentran y mantienen el rastro de que backends hay en ese set?.

Aquí es donde entran los **Servicios**.

Un servicio de Kubernetes es una abstracción que define un conjunto lógico de Pods y una política de acceso a estos - a veces llamados micro-servicios. El conjunto de pods apuntados por un servicio son (normalmente) determinados por un **Label Selector**.

Como ejemplo, consideremos un procesamiento de imagen en backend que se ejecuta con 3 réplicas. Estas réplicas son fungibles - el frontend no conocen qué backend utiliza. Mientras el Pod actual que compone ese backend puede cambiar, los clientes front-end no deberían tener que ser conscientes de eso o realizar un seguimiento de la lista de backends. La abstracción del servicio permite este desacoplamiento. 

Para las aplicaciones nativas de Kubernetes, Kubernetes ofrece una API de endpoints simple que se actualiza cada vez que cambia el conjunto de pods en un servicio. Para las aplicaciones no nativas, Kubernetes ofrece un puente basado en IP-virtual a los servicios que redirige a los pods de backend. 

Se usa un exposed. 

Un servicio se relaciona con un pod mediante selector, que son las etiquetas. Si hacemos una replica de un pod y tiene las mismas etiquetas que el pod exposeado al servicio, no hará falta hacerlo también con el segundo, ya que con las labels del selector es suficiente. 
La conectividad se comprueba con un curl. 

Unos service de loadbalancer puede tener ip interna y externa. Para mirar la conectividad se suele tener en cuenta la cluster-ip. nginx suele mostrar 404 por defecto, pero no hay problema. 

Hay que comprobar al interconexión para descartar fallos entre pods, si no es así entonces es problema del frontend o que esté mal exposeado.

nodePort, port y targetPort. 
El tráfico entra por el nodePort, puertos del propio nodo abierto para que escuche, y el puerto es el que escucha y recibe información el servicio; el targetPort es el propio del nodo. 

Cuando intentamos hallar la interconectividad de un pod y un service deberíamos mirar el targetPort.  
