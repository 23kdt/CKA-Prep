Nos permiten dejarlos en un determinado sitio dentro de nuestro contenedor como si fuera un fichero, para poder utilizarla posteriormente dentro de nuestro contenedor. 

Hemos creado este configmap. 

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: config-volumen
  namespace: default
data:
  ENTORNO: "desarrollo"
  VERSION: "1.0"
```

Ahora, si lo queremos utilizar dentro de un pod, debemos definirlo mediante:

```
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:
  containers:
    - name: contenedor1
      image: x
      command: ["/bin/sh", "-c", "sleep 100000"]
      volumeMounts:                             ####################
      - name: volumen-config-map
        mountPath: /etc/config-map               
  volumes:
    - name: volumen-config-map
      configMap:
        name: config-volume
  restartPolicy: Never
```

