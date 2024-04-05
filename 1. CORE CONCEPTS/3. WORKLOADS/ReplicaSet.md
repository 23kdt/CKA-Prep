Un controlador ReplicaSet se asegura de que un número específico de replicas de pods estén ejecutadose en cualquier tiempo dado.

Mientras que los ReplicaSets pueden ser usados individualmente, son principalmente utilizados por **Deployments** como mecanismos para orquestar la creación, actualización y eliminación de pods. Cuando usamos Deployments no tenemos que preocuparnos por como manejar los ReplicaSets. Los Deployments poseen y administran sus ReplicaSets.

Con el parametro ``replicas=x`` determinamos el número de réplicas que queremos para un pod, deployment, etc. 

Su proposito es mantener un número estable de réplicas de un pod, pero siempre se utilizan con deployments. 

Se configuran mediante ficheros YAML también. 


Un deployment lleva por debajo su propio replicaset. 

Cuando creamos un replicaSet mediante un deploy, el nombre del replicaSet será igual que el nombre del deploy más un número aleatorio. Ej deploy-12312903

``kubectl get rs`` 