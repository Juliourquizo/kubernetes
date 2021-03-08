# Práctica kubernetes

## Prerequisitos

1. Instalar **uno** de los siguietes softwares de Virtualización
    * Descargar e instalar VMware Workstation 16 Player. [Descargar](https://www.vmware.com/es/products/workstation-player/workstation-player-evaluation.html)
        * Si se pide la instalación de WMware Tools hacerlo.
    * Descargar e instalar VirtualBox 6.1.8. [Descargar](https://www.virtualbox.org/wiki/Downloads)
        * Instalar las Guest Additions cuando arranquemos el sistema.
2. Descargar la imagen de Ubuntu Desktop 20.04 LTS. [Descargar](https://releases.ubuntu.com/20.04/)
3. Instalar Ubuntu en una máquina virtual:
   * Mínimo 2 CPU, 6GB de RAM y 50 GB de disco duro   
4. Comprobar si GIT esta instalado, si no:
```bash
  sudo apt-get update
  sudo apt-get install git
```
5. Instalar Visual Studio Code (desde el centro de software de Ubuntu)
6. Instalar el binario de Terraform. [Guía](https://github.com/evtsrc/cloud/blob/main/README.md)
7. Instalar asciinema. [Guía](https://asciinema.org/docs/installation)
8. Instalar Docker. [Guía](https://docs.docker.com/engine/install/ubuntu/).
   * Si se quiere ejecutar el comando `docker` sin sudo seguir el paso 2 del siguiente [manual](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-es). (reiniciar para que coja los cambios)
9. Instalar docker compose. [Guía](https://docs.docker.com/compose/install/)

Llegados a este punto tienes varias opciones opciones:

* [VMWare Worstation + KVM](#opcion-a)
* [VMWare Worstation + Docker](#opcion-b)
* [VirtualBox + Docker](#opcion-c)

## OPCION A

### Utilizar VMWare Worstation y virtualización anidada con KVM

10. Parar la máquina virtual y configurar la siguiente opción en la configuración de VMWare Workstation:
   
   ![alt text](./vmware_conf_1.png "VMWare Configuration")

11. Instalar kvm
```bash
# Ejecutar los siguientes comandos. Tienen que dar diferente de 0 ambos
egrep -c '(vmx|svm)' /proc/cpuinfo
egrep -c ' lm ' /proc/cpuinfo
# Instalación
sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
# Añadir usuario a los grupos de kvm
sudo adduser `id -un` libvirt
sudo adduser `id -un` kvm
# Ejecutar y comrpobar que no da error
virsh list --all
# Reiniciar
```
> Referencia: [link](https://help.ubuntu.com/community/KVM/Installation)

12. Instalar minikube. [Guía](https://minikube.sigs.k8s.io/docs/start/)
    * Utilizar paquete Debian. **Solo hasta el paso 2**
13. Instalar kubectl. [Guía](https://kubernetes.io/docs/tasks/tools/install-kubectl/). 
> **Ojo con las versiones en este último paso, leer bien la guía. 
> Para saber la versión de kubernetes instalada arrancar y fijarse en los mensajes.
> En el siguiente ejemplo se está utilizando Kubernetes 1.20.0 (símbolo de la ballena)** 
```bash
$ minikube start --driver=kvm2
😄  minikube v1.16.0 on Ubuntu 20.04
✨  Using the kvm2 driver based on user configuration
💾  Downloading driver docker-machine-driver-kvm2:
    > docker-machine-driver-kvm2....: 65 B / 65 B [----------] 100.00% ? p/s 0s
    > docker-machine-driver-kvm2: 13.52 MiB / 13.52 MiB  100.00% 12.24 MiB p/s 
💿  Downloading VM boot image ...
    > minikube-v1.16.0.iso.sha256: 65 B / 65 B [-------------] 100.00% ? p/s 0s
    > minikube-v1.16.0.iso: 212.62 MiB / 212.62 MiB [ 100.00% 19.98 MiB p/s 10s
👍  Starting control plane node minikube in cluster minikube
🔥  Creating kvm2 VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.20.0 on Docker 20.10.0 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

## OPCION B

### Utilizar VMWare Worstation y Docker

11. Instalar minikube. [Guía](https://minikube.sigs.k8s.io/docs/start/)
    * Utilizar paquete Debian. **Solo hasta el paso 2**
12. Instalar kubectl. [Guía](https://kubernetes.io/docs/tasks/tools/install-kubectl/). 
> **Ojo con las versiones en este último paso, leer bien la guía. 
> Para saber la versión de kubernetes instalada arrancar y fijarse en los mensajes.
> En el siguiente ejemplo se está utilizando Kubernetes 1.20.0 (símbolo de la ballena)** 
```bash
$ minikube start
😄  minikube v1.17.1 en Ubuntu 20.04
🎉  minikube 1.18.1 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.18.1
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

✨  Automatically selected the docker driver. Other choices: kvm2, ssh
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.20.2 preload ...
    > preloaded-images-k8s-v8-v1....: 491.22 MiB / 491.22 MiB  100.00% 20.17 Mi
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparando Kubernetes v1.20.2 en Docker 20.10.2...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```


## OPCION C

### Utilizar VirtualBox y Docker

11. Instalar minikube. [Guía](https://minikube.sigs.k8s.io/docs/start/)
    * Utilizar paquete Debian. **Solo hasta el paso 2**
12. Instalar kubectl. [Guía](https://kubernetes.io/docs/tasks/tools/install-kubectl/). 
> **Ojo con las versiones en este último paso, leer bien la guía. 
> Para saber la versión de kubernetes instalada arrancar y fijarse en los mensajes.
> En el siguiente ejemplo se está utilizando Kubernetes 1.20.0 (símbolo de la ballena)** 
```bash
$ minikube start
😄  minikube v1.17.1 en Ubuntu 20.04
🎉  minikube 1.18.1 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.18.1
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

✨  Automatically selected the docker driver. Other choices: kvm2, ssh
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.20.2 preload ...
    > preloaded-images-k8s-v8-v1....: 491.22 MiB / 491.22 MiB  100.00% 20.17 Mi
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparando Kubernetes v1.20.2 en Docker 20.10.2...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

13. Instalar entornos virtuales de python 

```bash
sudo apt-get install python3-venv
```
