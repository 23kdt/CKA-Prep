Un secret permite guardar información confidencial dentro de K8s, como contraseñas, tokens de autorización, claves SSH, etc.  Es básicamente un ConfigMap pero protegido.
Normalmente nos sirven como método de autenticación para acceder a distintos recursos protegidos dentro de k8s, desde los componentes internos hasta nuestras propias aplicaciones.
Disponemos de distintos tipos de secrets orientados a distintos recursos dentro de la infraestructura.
La forma más directa de utilizar estos Secrets dentro de k8s es asociandolos a un Pod. 

Tipos:
- **Opaques** (o genéricos): tipo por defecto que contiene cualquier información que queramos incluir y proteger. 
- **Service Account Token**: almacena un token que identifica un Service Account. Representan una identidad para los procesos que se ejecutan dentro de un Pod.
- **Docker config**: almacena las credenciales necesarias para poder acceder a un registro privado de Docker
- **Basic authentication**: credenciales que se utilizan para la autenticación básica y que contiene dos propiedades obligatorias: username y password.
- **SSH**: almacenan las credenciales para poder autenticarnos a través de SSH. Vamos a necesitar una propiedad denominada "ssh-privatekey" que en realidad es un par de clave-valor. 
- **TLS**: almacenan un certificado y su clave asociada que se utilizan habitualmente para TLS. Se suelen utilizar con un recurso de tipo ingress, es decir, para acceder desde el exterior del clúster. En este caso deberemos tener los valores "tls.key" y "tls.crt". 
- **Bootstrap**: tokens especiales y se utiliza para arrancar en los nodos de k8s. 

### OPAQUES (Genéricos)

La forma más simple es igual que con el configMap, añadiendo --from-literal. 
``kubectl create secret generic passwords --from-literal=pass-root=kubernetes --from-literal=pass-user=kubernetes``

Se pueden realizar el resto de casos que en [[DEVOPS/K8S/CURSO/VARIABLES, CONFIGMAPS, SECRETS/ConfigMaps y Volumenes]] y [[DEVOPS/K8S/CURSO/VARIABLES, CONFIGMAPS, SECRETS/ConfigMaps]], únicamente cambiando el env añadiendo valueFrom: secretKeyRef 


### SECRETS DECLARATIVOS
En los casos ateriores, el cifrado lo añadía automáticamente k8s, pero podemos indicarlo manualmente. 
