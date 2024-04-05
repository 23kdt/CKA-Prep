Es igual que el LoadBalancer pero el segundo se integra con el CloudController de un proveedor. 

Para exponer un deployment es igual que con un pod. 
``kubectl expose deploy <deploy_name> --port=80 --type=NodePort`` 

Si no indicamos el tipo por defecto será un ClusterIP. 

### Cómo sabe el servicio dónde tiene que ir

Esto es gracias a los endpoints que tiene el servicio. Este endpoint del servicio apunta a la dirección IP del servicio. Por debajo utiliza un selector que buscará el nombre del deploy en las etiquetas de los pods, que se crea de forma automática al desplegar un deploy. 

Cuando escalamos los pods de un deploy, los endpoints del servicio se actualizan automáticamente, añadiendo la dirección IP de los nuevos pods. 
El reasignado de IPs es automático, por lo que nos da igual si un pod se cae y se crea uno nuevo, ya que el cliente podrá acceder al pod dinámicamente. 


