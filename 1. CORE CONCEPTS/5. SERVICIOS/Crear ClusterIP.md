Es el servicio predeterminado. 
En el ejemplo, vamos a crear una base de datos Redis (para lecturas de caché en clúster de alta disponiblidad),maestro, y otro contenedor que será el cliente llamado Redis_CLI. 
Para conectar estos dos contenedores usaremos un servicio, por lo que si el cliente solicita información del maestro, deberá solicitarselo al servicio, que será un ClusterIP. 

``kubectl create deployment redis-master --image=redis``
``kubectl create deployment redis-cli --image=redis`` 

Para conectarmos al maestro deberemos exponer el deployment redis-master.

``kubectl expose deploy redis-master --port=6379 (--type=ClusterIP)``

Al ser un cluster IP no tendrá un puerto desde el que será accesible desde el exterior si hacemos un ``kubectl get svc``. 

Si accedemos a la bash del deploy redis-cli: 
``kubectl exec redis-cli-81912347-nnwbh -it -- bash``

Y desde su bash si hacemos: 
``redis-cli -h redis-master

El nombre del servicio funciona como nombre de máquina. 
-h es de host. 

Si añadimos una clave-valor en el master:
``set v1 10`` 

Podremos consultarla desde el redis-cli y desde el pod del redis-master. 
