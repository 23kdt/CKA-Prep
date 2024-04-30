# ARQUITECTURA DEL CLÚSTER

Usando una analogía de barcos y puertos para intentar reflejar en qué consiste Kubernetes así como sus componentes.

El propósito de K8s es alojar sus aplicaciones en forma de contenedores de forma automatizada para que pueda desplegar tantas instancias de su aplicación como sea necesario y permitir la comunicación entre los servicios de esta. 

Tenemos dos tipos de barcos: los buques de carga que transportan los contenedores por el mar , y los buques de control,  que gestionan y supervisan los buques de carga. 
Los worker nodes son barcos que pueden cargar los contenedores. Pero, algo tiene que cargar los contenedores en estos barcos y organizar como se descargan, hacer un seguimiento, inventariado y control. Para ello, se requiere de distintos equipos o departamentos.

De esto se encarga el nodo maestro del clúster de Kubernetes. Este nodo maestro se encarga de administrar, planfiicar, organizar y monitorizar la información relativa a diferentes nodos, planificando qué contenedores van a cada uno, etc.
Para ello, tiene un conjunto de componentes:
- **etcd**: base de datos que almacena información en formato clave-valor. [(2. ETCD)](2.%20ETCD.md)
- **kube-scheduler**: es el planificador que se encarga de identificar el nodo correcto para colocar un contenedor en función de los recursos de los contenedores, capacidad de los nodos o cualquier otra política o reglas de afinidad. [(5. KubeScheduler)](5.%20Kube%20Scheduler.md)
- **node-controller**: se encarga de los nodos. Se encarga de incorporar nuevos nodos al clúster, gestionar las situaciones en las que no están disponibles o se destruyen. [4. Kube Controller Manager](4.%20Kube%20Controller%20Manager.md)
- **replication-controller**: garantiza que el número deseado de contenedores se ejecute en todo momento en un ReplicationGroup. 

Todos estos componentes se engloban en la **kube-apiserver** [(3. Kube-API Server)](3.%20Kube-API%20Server.md), uno de los principales componentes de gestión de Kubernetes. Es el responsable de orquestar todas las operaciones dentro del clúster. Expone la API de Kubernetes que utilizamos para acceder al clúster desde el exterior, así como distintos controladores. 

Necesitamos que todo sea compatible con los contenedores, así que necesitamos un *container-runtime*. A partir de la versión 1.24 Docker ha dejado de ser compatible con K8s, y se utiliza generalmente container-d.  Por tanto, necesitamos tener este container-runtime instalado en todos los nodos del clúster. 

Volviendo a los buques de carga. Cada uno de estos barcos tiene un capitán que es el responsable de mantener el contacto con los barcos principales, empezando por hacer saber al capitán que están interesados en unirse al grupo, recibir información sobre los contenedores que se van a cargar en el barco y cargar los contenedores adecuados, enviar información al capitán sobre el estado de este barco y el estado de los contenedores, etc. Esto es el **kubelet**, agente que se ejecuta en cada nodo del clúster. Escucha las instrucciones del servidor de la API y despliega o destruye los contenedores en los nodos según sea necesario [(6. Kubelet)](6.%20Kubelet.md). 

El **kube-proxy** asegura que las reglas necesarias están en su lugar en los worker nodes para permitir que los contenedores que se ejecutan en ellos se comuniquen entre sí [(7. Kube-Proxy)](7.%20Kube-Proxy.md). 