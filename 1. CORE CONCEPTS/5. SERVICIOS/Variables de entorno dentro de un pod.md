Cuando se crea un servicio, dentro de cada pod se crean distintas variables de entorno que se integran con Kubernetes. 
Si nos metemos dentro de los pods asociados con los servicios, si hacemos un ``env | grep NGINX`` en la bash, nos mostrará las variables de entrono creadas al asociar un pod con un servicio con una imagen de NGINX. 
