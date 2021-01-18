# Práctica kubernetes

## Prerequisitos

1. Descargar e instalar VMware Workstation 16 Player. [Descargar](https://www.vmware.com/es/products/workstation-player/workstation-player-evaluation.html)
   * Si se pide la instalación de WMware Tools hacerlo. 
2. Descargar la imagen de Ubuntu Desktop 20.04 LTS. [Descargar](https://releases.ubuntu.com/20.04/)
3. Instalar Ubuntu en una máquina virtual:
   * Mínimo 2 CPU, 6GB de RAM y 50 GB de disco duro
4. Comprobar si GIT esta instalado, si no:
```bash
  sudo apt-get update
  sudo apt-get install git
```
5. Instalar Visual Studio Code (desde el centro de software de Ubuntu)
6. Bajar e instalar el binario de Terraform. [Descargar](https://www.terraform.io/downloads.html)
7. Instalar Docker. [Guía](https://docs.docker.com/engine/install/ubuntu/).
   * Si se quiere ejecutar el comando `docker` sin sudo seguir el paso 2 del siguiente [manual](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-es). (reiniciar para que coja los cambios)
8. Instalar docker compose. [Guía](https://docs.docker.com/compose/install/)
9. Instalar minikube. [Guía](https://minikube.sigs.k8s.io/docs/start/)
    * Utilizar paquete Debian. **Solo hasta el paso 2**
10. Instalar kubectl. [Guía](https://kubernetes.io/docs/tasks/tools/install-kubectl/). 
> **Ojo con las versiones en este último paso, leer bien la guía. 
> Para saber la versión de kubernetes instalada arrancar y fijarse en los mensajes
> En el siguiente ejemplo se está utilizando Kubernetes 1.20.0** 
```bash
$ minikube start
😄  minikube v1.16.0 on Ubuntu 20.04
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.20.0 on Docker 20.10.0 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```
