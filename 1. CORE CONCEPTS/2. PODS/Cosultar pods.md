## Consultar pods

``kubectl get pods 

Si ponemos el parámetro -A nos muestra todos los pods de todos los namespaces. 
Si no añadimos ningún parámetro, nos muestra los pods del namespace por defecto.
Podemos enfocarnos en un namespace concreto mediante:

``kubectl get pods -n (namespace) 

``kubectl get pods -namespace (namespace)

Los pods también funcionan con etiqueta. Con el siguiente comando observamos las etiquetas de cada pod:

``kubectl get pods --show-labels 

Luego podemos consultar los pods por etiqueta mediante:

``kubectl get pods -A --selector run=(etiqueta)


## DESCRIBE 

Con **describe** tenemos más información sobre los pods:

``kubectl describe pod (nombrepod)``

Nos muestra ids, imagenes, puertos, estado, ips, etiquetas, condiciones, volúmenes, etc. 

Nos facilita también los eventos del pod, mostrandonos para cada uno de ellos el tipo, razon (scheduled, pulling, pulled, created, started, etc.), tiempo de vida, elemento que lo creó (scheduler, kubelet, etc. ) o un mensaje. 

### Memoria

En la sección de limits y request, tenemos la memoria asignada y la ocupada respectivamente. Normalmente se superan en memoria y es un error común, el pod se reinicia si ha sido un pico de memoria. Sino puede ser un error de una actualización del software del pod o un incremento sustancial en el número de clientes de este.

**IMPORTANTE** porque muchas veces es una causa de fallo sobreexceder la memoria asignada por el scheduler. 

Si sobreexcede la memoria asignada, queda en estado out-of-memory-kill y se reiniciaría el contenedor. El límite se puede definir tanto a nivel de pod como de contenedor. 

``kubectl top pod (nombrepod) -n namespace`` nos muestra qué memoria está usando el pod directamente. Es una herramienta externa a kubectl, hay que instalarla. 


## LOGS
Con ``kubectl logs (nombre_pod) -n namespace``  observamos los logs de los pods (únicamente se pueden consultar los logs de estos)

