# ACCIONES SOBRE UN POD

## Consultar pods

```shell
kubectl get pods 
```

Si ponemos el parámetro -A nos muestra todos los pods de todos los namespaces. 
Si no añadimos ningún parámetro, nos muestra los pods del namespace por defecto.
Podemos enfocarnos en un namespace concreto mediante:

```shell
kubectl get pods -n (namespace) 
```

```shell
kubectl get pods -namespace (namespace)
```

Los pods también funcionan con etiqueta. Con el siguiente comando observamos las etiquetas de cada pod:

```shell
kubectl get pods --show-labels 
```
Luego podemos consultar los pods por etiqueta mediante:

```shell
kubectl get pods -A --selector run=(etiqueta)
```

## DESCRIBE 

Con **describe** tenemos más información sobre los pods:

```shell
kubectl describe pod (nombrepod)
```

Nos muestra ids, imagenes, puertos, estado, ips, etiquetas, condiciones, volúmenes, etc. 

Nos facilita también los eventos del pod, mostrandonos para cada uno de ellos el tipo, razon (scheduled, pulling, pulled, created, started, etc.), tiempo de vida, elemento que lo creó (scheduler, kubelet, etc. ) o un mensaje. 

### Memoria

En la sección de limits y request, tenemos la memoria asignada y la ocupada respectivamente. Normalmente se superan en memoria y es un error común, el pod se reinicia si ha sido un pico de memoria. Sino puede ser un error de una actualización del software del pod o un incremento sustancial en el número de clientes de este.

**IMPORTANTE** porque muchas veces es una causa de fallo sobreexceder la memoria asignada por el scheduler. 

Si sobreexcede la memoria asignada, queda en estado out-of-memory-kill y se reiniciaría el contenedor. El límite se puede definir tanto a nivel de pod como de contenedor. 

```shell
kubectl top pod (nombrepod) -n namespace
``` 
Nos muestra qué memoria está usando el pod directamente. Es una herramienta externa a kubectl, hay que instalarla. 


## LOGS
Con el comando:
```shell
kubectl logs (nombre_pod) -n namespace
```  
Observamos los logs de los pods (únicamente se pueden consultar los logs de estos)

---

## EJECUTAR UN POD

La sintaxis para ejecutar un pod es: 

```shell
kubectl exec -it (nombrepod) -- /bin/bash 
```

**Importante los espacios después de los --**. El comando después de los guiones indicará el comando a ejecutar en el pod. 


De esta forma estaremos dentro del contenedor.  Si el pod tiene varios contenedores debemos especificar cuál de ellos queremos ejecutar:

```shell
kubectl exec -it (nombrepod) -C (contenedor) -- /bin/bash 
```


En el ejemplo del video2, accedemos al pod creado con un deployment mediante:
```shell
kubectl exec -it ejercicios2-57ff5c487d-k6nnq -n ddoradog -- /bin/bash
```

## REINICIAR PODS

Podemos indicarlo mediante una restart policy, que indicará los pasos a seguir en caso de que el pod se reinicie. Hay matices, pueden aplicarse en función del tipo de parada: Always, OnFailure, Never; básicamente, estos parámetros se ejecutarán si se realiza cualquier tipo de parada (por defecto), parada en caso de error (por ejemplo, si es un reinicio programado o una parada del clúster manual no debe aplicarse) o en ningún caso, por lo que las politicas de reinicio quedarían desactivadas. 

## REALIZAR PORT-FORWARDING CON EL POD
Esto se realiza con:

```shell
kubectl port-forward <nodename> <ext_port:int_port>```
```

Por ejemplo:
```shell
kubectl port-forward nginx1 9999:80
```

El kubectl lo único que hará será redirigir todas las solicitudes que vayan al puerto 9999 al puerto 80 del pod. 

## PROBAR POD CON UN PROXY

Si hacemos un ``kubectl proxy`` nos mostrará una ip que mediante el puerto 8001 todas las APIs que tiene Kubernetes. 

Si buceamos entre las apis podemos ver todos los componentes. 
Por ejemplo, si ponemos ``localhost:8001/api/v1/namespaces``, veremos todos los namespaces de nuestro clúster.

Y si hacemos ``http://127.0.0.1:8001/api/v1/namespaces/default/pods/nginx1/proxy/`` observaremos cómo nuestro pod creado con ``kubectl run nginx1 --image=nginx`` tiene conexión. 

