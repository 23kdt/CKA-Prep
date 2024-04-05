Igual que nodePort pero solo tiene sentido si trabajamos con Cloud, ya que es exactamente igual. La única diferencia es que añadirá una IP externa para que pueda ser accesible desde fuera del clúster. Se utiliza el cloudController del proveedor externo para acceder a esta IP, por lo que si desplegamos un LoadBalancer sin trabajar con Cloud, no se podrá acceder a esa external IP y el comportamiento será igual que en un NodePort.

``kubectl expose deploy apache2 --port=80 --type=LoadBalancer``

Si no está asociado con un proveedor cloud, la external IP seguirá estando en pending. 

