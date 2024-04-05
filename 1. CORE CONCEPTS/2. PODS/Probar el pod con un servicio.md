Toda la información estará más detallada en [[PRÁCTICAS/K8S/Servicios]].

Podemos exponer el recurso de un clúster mediante exposed:

``kubectl expose pod <namepod> --port=80 --name=nginx1-svc --type=LoadBalancer``

El puerto es por el que escucha el pod. Todos los pod con nginx escuchan por el puerto 80, por lo que este puerto será el puerto interno del propio contenedor. 
El nombre si no se indica será el del propio pod. 
Al indicar el tipo del pod podemos hacer que sea accedible desde fuera del clúster. Un pod es interno, por lo que en teoría solo sería accesible de forma interna. Con el parámetro ``-type=LoadBalancer`` logramos esto.

- **DockerDesktop no soporta este servicio de LoadBalancer, por lo que deberemos indicar "NodePort"**

Si hacemos un ``kubectl get svc`` y observamos el servicio creado, veremos como en la columna de Puertos se indican 2 puertos distintos. Uno es el interno, como hemos mencionado antes; el otro será el puerto de acceso al LoadBalancer, de forma que, si se realiza una solicitud del exterior a dicho puerto, se redirigirá al puerto 80 del pod en cuestión. Este puerto es a partir del 30000 y se asignan automáticamente. 

Una representación podría ser la siguiente:

![[DEVOPS/K8S/anexo/Pasted image 20230314154959.png]]

