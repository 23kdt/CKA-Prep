Los endpoints también son objetos de kubernetes, no solo una propiedad de los servicios. 
Si hacemos
``kubectl get endpoints`` 
Nos mostrará los servicios con sus respectivos endpoints. 
Si hacemos un describe sobre el endpoint también nos mostrará información. 

Pero, si consultamos su archivo yaml nos mostrará aún más información:

``kubect get endpoinst <nombre> -o yaml``

Nos muestra también la ip, nodo al que está asignado, información del target, etc. 

