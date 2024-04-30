Ficheros que nos van a permitir incorporar un conjunto de propiedades-valor, de forma que no tengamos que intorducirlos todos de forma manual.

Se adjuntan sobre un pod añadiendo en su manifiesto yaml:

```
spec:
  containers:
    - name: test-container
      image: x
      envFrom:
      - configMapRef:                             #ESTO
          name: cf1
```

Se pueden crear de forma imperativa:
``kubectl create configmap cf1 --from-literal=usuario=diego --from-literal=password=pass1``

Los configmaps se consultan con:
``kubectl get cm cf1 -o wide`` o ``kubectl describe cm cf1``

Otra opción será crear un configmap a partir de un fichero con el conjunto de variables clave-valor:
``kubectl create configmap cf2 --from-file= datos_mysql.properties``

De esta forma se definirá como clave el nombre del fichero, y todas las variables se quedarán como valor. Cuando hacemos esto, se añaden al configmap como si fuesen un único valor. Para variables de entorno no vale, pero puede ser útil para otras funciones. 


Sin embargo, lo podemos añadir como si el archivo de referencia fuera un archivo de variables de entorno, lo que nos permitiría solucionar lo anterior:
``kubectl create configmap cf3 --from-env-file datos_mysql.properties``

Aquí ya si se tomarán como x entradas y no como solo 1, y si lo consultamos en las variables de entorno del pod veremos cómo ya aparecen distintas variables de entorno y no sólo 1. 

---

Los **configmaps** son objetos de la API de kubernetes utilizados para almacenar datos no confidenciales en formato clave-valor. Nos sirve para almacenar datos y utilizarlo en pods.

La utilidad de los configmaps es que los pods pueden utilizarlos como variables de entorno, argumentos de línea de comandos o como ficheros de configuración. 

Podemos crear configmaps de forma imperativa (línea de comandos) o declarativa, a través de un fichero YAML. 

Hay tres formas de agregar contenido a un configmap cuando se crea, que son:
1. Agregando un fichero como contenido.
2. Agregando el contenido de un fichero.
3. Agregando directamente los pares claves:valor. 

De forma imperativa se crea:

``kubectl create configmap (configmap_naem) --from--file=(fichero_o_directorio) -n namespace

o su shortcut cm ( ``  kubectl create cm ... ``)

De forma declarativa, sería modificando el fichero YAML. 
En concreto, debemos modificar la sección "data". A la izquierda tendremos el nombre del fichero, un pipe ( "   |   "), y a la derecha su contenido. 


Para crear un configmap agregando los pares clave:valor, directamente podemos utilizar el comando:

`` kubectl create configmap (configmap_name) --from-literal=(key)=(value)

Podemos agregar el parámetro --from-literal tantas veces como deseemos, pero si se van a agregar múltiples variables es recomendable usar un fichero. 
El YAML de un configmap creado utilizando este comando, se verá igual que los vistos en el video. 

### Configurando configmaps en contenedores

Se pueden configurar en contenedores de las siguientes formas:

- Como variables de entorno de un contenedor

Mediante la etiqueta env:
![[Pasted image 20230310122217.png]]


- Configurando todos los pares clave:valor de un configmap como variables de entorno de un contenedor. 
- Utilizar configmaps definidos como variables de entorno en comandos del pod. 
- Añadir un configmap a un volúmen (más complicada)

