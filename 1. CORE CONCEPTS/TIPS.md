# COMANDOS RÁPIDOS
En los exámenes, una buena opción para tener una plantilla de manifiesto es hacer uso de kubectl run, en lugar de copiar y pegar un fichero YAML.

### **Create an NGINX Pod**

```shell
kubectl run nginx --image=nginx
```

### **Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)**

```shell
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

### **Create a deployment**

```shell
kubectl create deployment --image=nginx nginx
```

### **Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)**

```shell
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

### **Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file.**

```shell
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

### **Make necessary changes to the file (for example, adding more replicas) and then create the deployment.**

```shell
kubectl create -f nginx-deployment.yaml
```
**O bien, en la versión k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.**

```shell
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`
```