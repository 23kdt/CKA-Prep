
La sintaxis para ejecutar un pod es: 

``kubectl exec -it (nombrepod) -- /bin/bash ``

**Importante los espacios después de los --**. El comando después de los guiones indicará el comando a ejecutar en el pod. 


De esta forma estaremos dentro del contenedor.  Si el pod tiene varios contenedores debemos especificar cuál de ellos queremos ejecutar:

``kubectl exec -it (nombrepod) -C (contenedor) -- /bin/bash ``


En el ejemplo del video2, accedemos al pod creado con un deployment mediante:
``kubectl exec -it ejercicios2-57ff5c487d-k6nnq -n ddoradog -- /bin/bash
