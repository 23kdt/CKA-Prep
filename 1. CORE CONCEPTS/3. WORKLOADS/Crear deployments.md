Al igual que con los pods, podemos crear deployments de forma imperativa o de forma declarativa, siendo nuevamente recomendable usar la segunda opción.

``kubectl create deployment apache --image=httpd`` 

El comando anterior lanzará un deployment con un único pod. 
Al crear un deploy también se creará un replicaSet conjunto enlazado a este. 

Cuando creamos un replicaSet mediante un deploy, el nombre del replicaSet será igual que el nombre del deploy mas un número aleatorio. Ej deploy-12312903

``kubectl get rs`` 

El nombre del pod/pods creados en el deployment, tendrán el nombre del replicaSet mas un número aleatorio. Ej. deploy-12312903-1282412

