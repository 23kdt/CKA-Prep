Si hacemos un ``kubectl proxy`` nos mostrará una ip que mediante el puerto 8001 todas las APIs que tiene Kubernetes. 

Si buceamos entre las apis podemos ver todos los componentes. 
Por ejemplo, si ponemos ``localhost:8001/api/v1/namespaces``, veremos todos los namespaces de nuestro clúster.

Y si hacemos ``http://127.0.0.1:8001/api/v1/namespaces/default/pods/nginx1/proxy/`` observaremos cómo nuestro pod creado con ``kubectl run nginx1 --image=nginx`` tiene conexión. 

