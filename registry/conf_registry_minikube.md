# Utilizar un registry dentro de minikube

1. Activar el addon de registry que trae minikube
```bash
minikube addons enable registry
```
2. Averiguar la ip con la que está levantado minikube

```bash
minikube ip
```

3. Modificar/crear el fichero del daemon de docker para aceptar conexiones a registros inseguros, configurandolo con la ip de minikube

```bash
sudo vi /etc/docker/daemon.json 
```
```json
{
"insecure-registries" : ["valor_de_minikube_ip:5000"]
}
```

4. Reiniciar el servicio de docker

```bash
sudo systemctl restart docker.service
```

5. Parar  minikube y arrancarlo añadiendo el parámetro de insecure-registry con una ip y máscara que nos permita conectarnos con el registry interno (ten en cuenta que la ip y la máscara utilizadas tienen que ser coherentes con el valor obenido en el comando `minikube ip`)

```bash
minikube stop
# Si utilizas kvm
minikube start --driver=kvm2 --insecure-registry "192.168.39.0/24"
# Si utilizas docker
minikube start --insecure-registry "192.168.39.0/24"
```

6. Para subir una imagen al registry tendremos que hacer un tag de nuestra imagen con la ip de minikube y el puerto de conexión. Después podemos hacer el push

```bash
docker tag nombre_imagen minikube_ip:5000/nombre_imagen
docker push http://minikube_ip:5000/nombre_imagen
```

7. Desde los ficheros yaml habrá que hacer referencia a la imagen con `localhost:5000/nombre_imagen`

> **NOTA IMPORTANTE:** El registry no es persistente, por lo que cada vez que pares minikube se perderá la imagen subida. Tendrás que volverla a subir cada vez que arranques el cluster de minikube
