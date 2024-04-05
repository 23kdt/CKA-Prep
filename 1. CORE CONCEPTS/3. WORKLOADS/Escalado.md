## Escalado

El comando `` kubectl scale deploy <nombre_deploy> --replicas=x `` nos permite escalar el deployment sin modificar manualmente el manifiesto yaml.


Otra opción que podemos hacer con scale es modificar aquellos deployment mediante el uso de selectors y etiquetas:



```shell
kubectl scale deploy -l estado=1 --replicas=2
```

Este comando modificará aquellos deployment que tengan una etiqueta estado=1.

Debemos tener consideraciones sobre el escalado y la capacidad de replicación. No es lo mismo replicar un servicio frontal que un servicio de MySQL. Si lo hacemos con MySQL, cada servicio tendrá asociada una base de datos, por lo que si replicamos tendremos 2 servicios pero también dos bases de datos independientes, lo que no se debe hacer. Tampoco se puede utilizar un volumen compartido, ya que MySQL no soporta varios motores para un mismo volúmen.

No podremos replicar servicios con estado, pero hay herramientas especializadas como MySQL-Cluster o MySQL-Galera.


El problema de Kubernetes es que no gobierna lo que hay dentro de los clúster, eso depende de la aplicación. Kubernetes podrá replicar todos sus componentes, pero podrá ser o no funcional dependiendo del servicio que el cliente incluya dentro.