Se definen con el bloque env en el manifiesto yaml. 
Se establecen mediante clave valor.

```
  containers:
  - name:  var-ejemplo
    image: gcr.io...
    env:
    - name:  NOMBRE
      value: "CURSO DE K8S"
    - name:  PROPIETARIO
      value:  "kdt"
```

Estas properties o variables podrán ser accesibles desde dentro del contenedor. K8s lo único que hace es pasar las variables directamente al contenedor o recurso dónde lo definamos. 
Si nos metemos dentro del pod con una bash, podemos ver todas las variables de entorno mediante **printenv**, y observaremos cómo aparecen las variables definidas fuera están también dentro de las variables de entorno del pod. 
