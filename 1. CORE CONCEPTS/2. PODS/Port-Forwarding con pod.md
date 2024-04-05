
Esto se realiza con:

``kubectl port-forward <nodename> <ext_port:int_port>``. Por ejemplo:

``kubectl port-forward nginx1 9999:80``

El kubectl lo único que hará será redirigir todas las solicitudes que vayan al puerto 9999 al puerto 80 del pod. 

