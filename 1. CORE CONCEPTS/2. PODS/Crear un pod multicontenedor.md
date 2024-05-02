# Crear Pod multicontenedor

Supongamos que queremos tener un pod con un contenedor con un servicio web nginx y otro container que lo monitorice.

Un archivo del pod podría ser el siguiente. Por un lado tenemos el servicio web y por otro lado el  frontal con alpine (distribución de Linux reducida) y que ejecutará el comando watch y ping sobre localhost, que al compartir la dirección IP entre contenedores, se realizará sobre la IP del pod y por tanto a nginx. 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi
spec:
  containers:
  #servidor web
  - name: web
    image: nginx
    ports:
    - containerPort: 80
  #frontal de monitorización
  - name: frontal
    image: alpine
    command: ["watch", "-n5", "ping",  "localhost"]

```

Si hacemos un ``kubectl apply -f multi.yaml`` se creará el pod con los distintos contenedores. Si hacemos un get pods veremos como aparecen 2/2 en estado READY, lo que nos indica que el pod tiene 2 contenedores. 
Si por ejemplo realizamos un ``kubectl logs -f multi -c frontal`` para indicar que queremos ver los logs únicamente del contenedor frontal, observaremos como está haciendo ping sobre la ip del pod. 