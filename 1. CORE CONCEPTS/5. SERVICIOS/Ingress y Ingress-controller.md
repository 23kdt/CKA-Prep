

## INGRESS

Son la forma de exponer realmente los dominios. 


Objeto que expone las rutas HTTP y HTTPS desde fuera del clúster a los servicios dentro del clúster. Es en este objeto donde se definen las reglas que van a enrutar el tráfico entrante. 

![Imagen](/CKA-Prep/ANEXO%20IMAGENES/Pasted%20image%2020230314161525.png)

El servicio es de tipo clusterIP. 
Cuando la petición llega, el balanceador la introduce dentro del clúster, y será el ingress quien determinará las reglas de reenvío, que indicará a la petición lo que tiene que hacer y dónde ir.

Las principales formas de enrutar tráfico que nos permite un ingress son: 

- A través de paths: las reglas de tipo Path nos permiten indicar una ruta a partir del contexto raíz de nuestro aplicativo "/", para exponer un servicio a través de esta. 
- A través de un hostname: las reglas de tipo hostname, nos permiten indicar un nombre de dominio y exponer un servicio a través de este.

Un ejemplo de Ingress con reglas tipo path es: 

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	name: video-ingress
spec:
	rules:
	- http:
		paths:
		- path: /videos
		  pathType: Prefix
		  backend:
			  service:
				  name: videos-svc
				  port: 
					number: 80

```

Un ejemplo de Ingress con reglas de hostname:

```

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	name: ingress-wildcard-host
spec:
	rules:
	- host: "foo.bar.com"
	  http:
		paths:
		- pathType: Prefix
		  path: ""/bar"
		  backend:
			  service:
				  name: service1
				  port: 
					number: 80

```



##### **COMANDOS ÚTILES

- ``kubectl get ing -n namespace
- ``kubectl get ing -A
- ``kubectl describe ing -n namespace
- ``kubectl get ing -n namespace -oyaml
- ``kubectl explain ing -n namespace
- ``kubectl edit ing -n namespace
- ``kubectl delete ing -n namespace



## INGRESS-CONTROLER

Para poder utilizar los objetos del tipo ingress, es necesario disponer de un ingress-controller. Un ingress-controller no es más que un balanceador de carga especializado para K8S. 
Para desplegar un ingress-controller hay numerosas soluciones, una de las más utilizadas es **ingress-nginx**.

En Azure por ejemplo no me crea otro balanceador, sino me otorga una IP pública.

