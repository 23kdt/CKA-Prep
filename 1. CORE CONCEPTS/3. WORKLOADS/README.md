# CONTROLADORES 
Los pods tienen problemas de escalabilidad, no se recuperan ante caídas, son individuales, son complicados para hacer updates y rollbacks. 
La solución a esto es un deployment, una especie de burbujas que nos permite solucionar todos los problemas anteriores. 
Dentro de un deployment podemos crear distintos endpoints para un mismo servicio para replicar una funcionalidad y ofrecer disponibilidad y escalabilidad. 
Cuando creo un deployment crea a su vez un replicaSet, componente que se encarga de mantener una unidad fija de pods dentro de un deployment. Son independiente de los deployment, pero suelen utilizarse generalmente junto a un deployment para ofrecer un servicio. 


## WORKLOADs
Un workload es algo que utilizamos para desplegar contenedores. Son los que se encargan del trabajo pesado. Los pods, por ejemplo, son workloads. Envoltura para uno o varios pods haciendo que se ejecuten de alguna manera, pero siempre respetando el estado ideal.

Pero, hay muchos más tipos de workloads:

- Los **deployment** envuelven a los pods y les otorgan una serie de características, que también es un workload.
- Los **replicaSet** son otro tipo de workload, y que se encargan del escalado de pods comparando el estado deseado con el actual, aumentando o disminuyendo réplicas (también se llaman replicationController).
- Un **statefulSet** es un elemento que garantiza el orden y unicidad de los pods. 
- Un **daemonSet** es un componente que asegura que todos los nodos de un clúster van a tener una copia de un pod. 
- Los **jobs** son un componente que va a crear uno o varios pods y que se encargará de que x pods terminen correctamente. 
- El **cronJob** es igual pero de forma planificada. 

Un **controller se encarga de comprobar y controlar que el clúster y workloads se encuentran en un estado correcto. 