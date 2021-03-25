# Borrar imagen del registry

En caso de que quieras borrar una imagen subida al registry interno, aquí tienes unas pautas a seguir. Otra vez, ten en cuenta que estos comandos tienen la ip de un ordenador que no es el tuyo. Tendrás que adaptarlos a tu "situación".

## Uso del API del registry

Borrar la imagen mediante el uso del API del registry

```bash
curl http://192.168.39.178:5000/v2/_catalog
curl http://192.168.39.178:5000/v2/<name>/tags/list
curl -v --silent -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X GET http://192.168.39.178:5000/v2/<name>/manifests/latest 2>&1 | grep Docker-Content-Digest | awk '{print ($3)}'
curl -v -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X DELETE http://192.168.39.178:5000/v2/<name>/manifests/sha256:72efe8582aebe01c94c0b7b778fe65595633b19a11b2b8912146d7ff4d1e7f15
``` 

## Lanzar el recolector de basura

Entrar en el contenedor de docker del registry, crear el fichero `kk.yaml` y ejecutar el comando del recolector de basura. Por cierto, por si no había quedado claro, ten en cuenta que estos comandos tienen puesto el id de la imagen en el caso de un ordenador en concreto, sustituye el id con el que corresponda de tu instalación:

```bash
eval $(minikube docker-env)
docker exec -it 80f50e8606c7 /bin/sh
bin/registry garbage-collect kk.yaml -m
```

`kk.yaml`

```yaml
version: 0.1
log:
  fields:
  service: registry
storage:
    cache:
        blobdescriptor: inmemory
    filesystem:
        rootdirectory: /var/lib/registry
    delete:
        enabled: true
http:
    addr: :5000
    headers:
        X-Content-Type-Options: [nosniff]
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
```