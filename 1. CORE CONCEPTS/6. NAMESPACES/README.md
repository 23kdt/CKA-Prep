# NAMESPACES

Son agrupaciones virtuales para dividir el clúster. El nombre de los recursos/objetos creados dentro de un Namespace son únicos, pero no en los Namespaces del cluster.

Generalmente, Kubernetes crea cuatro Namespaces por defecto:

- **kube-system**: Contiene los objetos creados por el sistema, principalmente los agentes del plano de control.
- **kube-public**: Es un namespace inseguro y legible por cualquiera, utilizado para la exposición de información pública no sensible sobre el cluster.
- **default**: Contiene los objetos y recursos creados por los administradores y desarrolladores.