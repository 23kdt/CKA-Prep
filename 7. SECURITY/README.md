# Primitivas de seguridad

La autenticación basada en password debe estar deshabilitada, únicamente tiene que estar habilitada mediante SSH. 

El kube-apiserver es el centro de todo.
Necesitamos tenemos que tener en mente dos preguntas: ¿Quién puede acceder? ¿Qué pueden hacer?

#### ¿Quién puede acceder? 
Está definido por los mecanismos de autenticación.
- Archivos - Usuarios y contraseñas
- Archivos - Usuario y tokens
- Certificados
- Proveedores externos de autenticación - LDAP
- Service Accounts

#### ¿Qué pueden hacer?
- Definidos por RBAC autotrización
- ABAC autorización
- Node autorización
- Webhook autorización.

Toda la comunicación con el clúster entre los distintos componentes como el ETCD, kube-controller-manager, scheduler, etc. se aseguran utilizando cifrado TLS. 

En lo que corresponde a la comunicación de las aplicaciones dentro del clúster, por defecto todos los pods pueden comunicarse con el resto. Se pueden restringir utilizando network policies. 

