EsEs la forma de conectarnos de forma externa a nuestros contenedores.
Hay clientes que pueden necesitar acceder a los pods de un deployment, que cada uno tendrá su IP, pero, no podremos conectarnos directamente con ellos porque no hay IP fija, no hay nombre fijo y no hay un puerto fijo. 
Si se cae un pod, el replicaSet creará otro pod con una nueva IP.

El servicio será el intermediario, que añade una IP fija, un nombre fijo y un puerto fijo. El servicio hace una lista dinámica de todos los pods y se los ofrece al cliente. 

Tipos de servicios:

- ClusterIP: hace que los pods sean accesibles desde dentro del clúster. 
- NodePort: accesible desde fuera del clúster, con puerto con el que se puede acceder de forma externa llamado nodeport. 
- LoadBalancer: implementa características de ambos, pero que se suele integrar con loadbalancers de terceros, como AWS, GCP, Azure, etc. 

El servicio detecta los distintos pods que se van creando mediante el uso de labels, selectors. 