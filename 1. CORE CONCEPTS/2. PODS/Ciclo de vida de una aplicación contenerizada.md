# CICLO DE VIDA DE UNA APLICACIÓN 

Nuestra aplicación debe convertirse en una imagen que se pueda desplegar en un contenedor. En docker es muy fácil, ya que se hace ``docker run`` y ya está. En k8s, tenemos dos formas de pasar esta imagen a nuestro clúster:

- Modo imperativo: cuando tenemos un clúster, desplega esta aplicación en un container con x réplicas mediante ``kubectl run``
- Modelo declarativo: utilizando manifiestos yaml, declarando el estado deseado de nuestra aplicación para pedirle al clúster que me lo cree. Este manifiesto se almacena en **etc.d** . K8s se encargará de comparar constantemente mediante controladores el estado deseado con el estado actual, así yo no tengo que preocuparme. 
