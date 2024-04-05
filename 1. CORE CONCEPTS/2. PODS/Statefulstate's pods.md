Es muy común ver aplicaciones que están consumiendo más memoria que los request, y si están superando estos límites, no se podrán desplegar. 

Un statefulstate es igual que un deployment pero para aplicacioenes con estados, como bases de datos etc. 
Cuando tienen nifi-0, nifi-1, nifi-2, etc. suelen ser pods que dependen del sts. 

Se puede ver el tipo de pod con get pod (name), en sección de owner reference. 
Hay un tipo de recursos que se importan con liberias (como por ejemplo en histio que genera un recurso que se llama histio, que pueden gener otro tipo de recuros).

--admin, porque en muchos cluster necesitamos privilegios de admin y no de usuarios. 
