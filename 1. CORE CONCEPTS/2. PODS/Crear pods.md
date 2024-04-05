### Forma imperativa (no recomendada)

La forma más simple de hacerlo es mediante, por ejemplo, ``kubectl run nginx1 --image=nginx`` . 


### CREAR PODS  MEDIANTE MANIFIESTOS(modo declarativo, recomendado)

Para crear un pod debemos hacerlo desde un fichero de configuración YAML. Por ejemplo, creamos un fichero yaml llamado nginx-pod.yaml en una carpeta /examples con el siguiente formato:

```

# Número de versión del api que se quiere utilizar

apiVersion: v1

# Tipo de fichero que se va a crear.

kind: Pod

# Aquí van los datos propios del pod como el nombre y los labels que tiene asociados para seleccionarlo

metadata:

    name: my-nginx

    # Especificamos que el pod tenga un label con clave "app" y valor "nginx"

    labels:

        app: nginx

# Contiene la especificación del pod

spec:

    # Aquí se nombran los contenedores que forman parte de este pod. Todos estos contenedores serían visibles por localhost

    containers:

        - name: nginx

          image: nginx

          ports:

            - containerPort: 80

    # Aquí se define la política de restauración en caso de que el pod se detenga o deje de ejecutarse debido a un fallo interno.

    restartPolicy: Always

```

Después, mediante el comando creamos dicho pod.:

``kubectl create -f example/nginx-pod.yaml``

 
SI hacemos un describe veremos toda la información relativa al pod: 

``kubectl describe pod (nombrepod) -n namespace


---

## Comando APPLY

Cuando tenemos que modificar un pod, lo podemos realizar mediante el comando ``kubectl apply``. Esto lo único que hará será comparar el estado deseado con el actual. Añadirá, borrará, modificará, etc. automáticamente en función del estado deseado, por lo que es la forma más óptima de modificar algún recurso. 
Ejemplo: 

``kubectl apply -f nginx.yaml``

