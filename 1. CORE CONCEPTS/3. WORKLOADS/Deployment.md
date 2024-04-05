## Escalado

El comando ``kubectl scale deploy <nombre_deploy> --replicas=x`` nos permite escalar el deployment sin modificar manualmente el manifiesto yaml. 

Otra opción que podemos hacer con scale es modificar aquellos deployment mediante el uso de selectors y etiquetas:

```shell
kubectl scale deploy -l estado=1 --replicas=2
```

Este comando modificará aquellos deployment que tengan una etiqueta estado=1.


Debemos tener consideraciones sobre el escalado y la capacidad de replicación. No es lo mismo replicar un servicio frontal que un servicio de MySQL. Si lo hacemos con MySQL, cada servicio tendrá asociada una base de datos, por lo que si replicamos tendremos 2 servicios pero también dos bases de datos independientes, lo que no se debe hacer. Tampoco se puede utilizar un volumen compartido, ya que MySQL no soporta varios motores para un mismo volúmen. 
No podremos replicar servicios con estado, pero hay herramientas especializadas como MySQL-Cluster o MySQL-Galera. 

El problema de Kubernetes es que no gobierna lo que hay dentro de los clúster, eso depende de la aplicación. Kubernetes podrá replicar todos sus componentes, pero podrá ser o no funcional dependiendo del servicio que el cliente incluya dentro. 


---

Un controlador de despliegue proporciona actualizaciones declarativas para Pods y ReplicaSets. 
Describimos un estado deseado en un objeto Deployment, y el controlador cambiará el actual estado al estado deseado. Podemos definir Deployments para crear nuevos ReplicaSets, o eliminar Deployments existentes y adoptar todos sus recursos con nuevos Deployments.

Los Deployment manejan los ReplicaSets para orquestar los ciclos de vida del Pod. 

Con el parametro ``replicas=x`` determinamos el número de réplicas que queremos para un pod, deployment, etc. 

Lleva un replicaSet por debajo. 

El deployment controller trata de mantener el desired state, creando, eliminando o reemplazando los pods por nuevas configuraciones. Por ejemplo, si eliminamos uno de sus pods, automáticamente creará otro pod nuevo. 

***

##### YAML Deployment

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

- **replicas**: número de pods réplica que el deployment tratará de mantener.
- **selector**: label usado para identificar los replica pods que serán administrados por el deployment. 
- **template**: plantilla que se usará para crear los replica pods.

***

## Casos de uso habituales

- Escalar de forma sencilla una aplicación cambiando el número de replicas. 
- Usar rolling updates para desplegar nuevas versiones de software o hacer rollback a versiones anteriores. 

***

#### Crear deployment

```shell
kubectl create deployment hello-node --image=k8s.gcr.io/choserver:1.4
```

#### Consultar deployments

```shell
kubectl get deployments
```


### Describe

```shell
kubectl describe deployment <nombre_deployment>
```


#### Crear un despligue con réplicas

```shell
kubectl create deployment prueba --image=nginx --replicas=3 --dry-run=client
```

En este caso, la ejecución únicamente se simula gracias a ``--dry-run``

Este caso anterior puede ser útil para exportar el fichero de configuración mediante -o YAML, para su posterior modificación. 

#### Cambiar número de réplicas

```shell
kubectl scale deployment (nombre) --replicas=x 
```

También se puede hacer mediante ``kubectl scale``

#### Controlar actualizaciones

 - Para cambiar por ejemplo la imagen del deployment. 

```shell
kubectl edit reployment (nombre)
```

- Indica cómo ha ido la última actualización sobre el deployment. 
```shell
kubectl rollout status deployment/dev-logger -n dev-logger
```

- Sin embargo, con el siguiente comando volvemos a la última actualización:
```shell
kubectl rollout undo deployment/prueba
```

- Incluso, podemos volver atrás en distintas actualizaciones mediante:

```shell
kubectl rollout undo deployment/prueba --to-revision=2
```

dónde en el parámetro --to-revision indicamos el número de actualizaciones atrás que queremos.


Mediante ``kubectl get all `` nos muestra los replicaSet en ejecución pero también los anteriores.

