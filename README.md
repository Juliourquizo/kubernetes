# Práctica kubernetes

## Prerequisitos

Descargar la imagen de Ubuntu para importarla en VirtualBox [Descargar]https://drive.google.com/file/d/1LxLfw0AXCPWd0Wgf4oimbP2AnwwnidlB/view?usp=sharing)

Una vez importada la imagen ejecutar `minikube start`

```bash
$ minikube start
😄  minikube v1.25.1 en Ubuntu 20.04 (vbox/amd64)
✨  Controlador docker seleccionado automáticamente
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Descargando Kubernetes v1.23.1 ...
    > preloaded-images-k8s-v16-v1...: 504.42 MiB / 504.42 MiB  100.00% 4.78 MiB
🔥  Creando docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparando Kubernetes v1.23.1 en Docker 20.10.12...
    ▪ kubelet.housekeeping-interval=5m
    ▪ Generando certificados y llaves
    ▪ Iniciando plano de control
    ▪ Configurando reglas RBAC...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Complementos habilitados: default-storageclass, storage-provisioner
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```
