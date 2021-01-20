# Práctica kubernetes

## Prerequisitos

1. Descargar e instalar VMware Workstation 16 Player. [Descargar](https://www.vmware.com/es/products/workstation-player/workstation-player-evaluation.html)
   * Si se pide la instalación de WMware Tools hacerlo. 
2. Descargar la imagen de Ubuntu Desktop 20.04 LTS. [Descargar](https://releases.ubuntu.com/20.04/)
3. Instalar Ubuntu en una máquina virtual:
   * Mínimo 2 CPU, 6GB de RAM y 50 GB de disco duro
   * Configurar también la siguiente opción:
   
   ![alt text](https://github.com/evtsrc/kubernetes/blob/main/vmware_conf_1.png "VMWare Configuration")
   
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
9. Instalar kvm
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

10. Instalar minikube. [Guía](https://minikube.sigs.k8s.io/docs/start/)
    * Utilizar paquete Debian. **Solo hasta el paso 2**
11. Instalar kubectl. [Guía](https://kubernetes.io/docs/tasks/tools/install-kubectl/). 
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
