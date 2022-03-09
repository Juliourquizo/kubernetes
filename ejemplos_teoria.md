# Ejemplos teoría

## Autocompletado
```bash
source <(kubectl completion bash)
```

## Namespaces

### Crear un namespace comando
```bash
kubectl create namespace first-namespace
```

### Crear namespace con fichero
```bash
kubectl create -f namespace.yaml
```

### Borrar namespace
```bash
kubectl delete namespaces namespace-from-yaml
kubectl delete namespaces first-namespace
```

## Pod

### Crear pod
```bash
kubectl apply -f new_nginx_pod.yaml
# Comprobar el estado
kubectl get pods -w
# Consultar su descripcion
kubectl describe pod new-nginx
```
> ¿Cómo puedo ver la imagen descargada? `eval $(minikube docker-env)`

### Crear pod en un namespace en concreto
```bash
# Crear el namespace
kubectl create namespace first-namespace
# Crear pod en namespace diferente a default
kubectl apply -f new_nginx_pod.yaml -n first-namespace 
# Consultar pods creados
kubectl get pods
kubectl get pods -n first-namespace
# Comprobar en el describe el campo namespace 
kubectl describe pod new-nginx
kubectl describe pod new-nginx -n first-namespace
# Limpieza
kubectl delete pod new-nginx
kubectl delete pod new-nginx -n first-namespace
kubectl delete namespaces first-namespace
```

### Borrar el pod
```bash
kubectl delete pod new-nginx
```

### :clipboard: Ejercicio 1

> Despliega un pod con un contenedor que se cree desde una imagen llamada busybox con el nombre `caja` en un namespace que se llame `spacebox`. 
> 
> Tiene que tener una variable de entorno que se llame `miVar` y que su valor sea "`Estoy dentro`". Además tiene que tener una etiqueda que sea `version` con el valor `0.1`
> 
> Intenta entrar dentro de ese contenedor para ver si se ha creado bien la variable de entorno.
> 
> **¡OJO!** La imagen de busybox "no hace nada", así que si no haces algo para que se mantenga arrancada, no conseguirás avanzar con el ejercicio. También tendrás que investigar cómo acceder al contenedor utilizando el comando `kubectl`.

## Deployment

### Crear deployment
```bash
kubectl apply -f nginx_deployment.yaml
kubectl get deployments -w
kubectl get pods -o wide
# Consultar el replicaset
kubectl get rs
kubectl describe rs nginx-deployment
```

**Uso de labels en la consulta**
```bash
kubectl get pods --show-labels
kubectl get pods -A
kubectl get pods -A -l app=nginx
```

### Limpieza
```bash 
kubectl delete deployment nginx-deployment
```

### :clipboard: Ejercicio 2

> Borra lo que hayas creado anteriormente. 
>
> Crear un deployment igual que el de nginx visto en clase pero con 2 réplicas y cambiando el selector para que la key sea `front` y el valor `end`. 
> 
> Cambia además la estrategia en la que se realizan los cambios de versión para que sea del tipo `recreate`.
>
> Una vez desplegado haz un cambio en la versión de la imagen de nginx para subir a la versión `1.20.2`. **No modifiques el yaml**, modifica lo que ya está desplegado. Para ello tendrás que utilizar el comando `kubectl set` añadiendo más parámetros. Cuando lo hayas conseguido fijate bien cómo reinician los pods ¿de uno en uno? ¿todos a la vez? Investiga las diferencias que existen entre una estrategia de rollout de tipo `Recreate` y uno de tipo `RollingUpdate`.
>
> Si has conseguido cambiarlo, intenta utilizar el comando [`kubectl rollout`](https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl_rollout/) para ver el historial de cambios del deployment e intenta volver a la versión anterior sin realizar ningún cambio.

## Servicios

### ClusterIP
```bash
kubectl create namespace test-servicios
kubectl apply -f hello_deployment.yaml -n test-servicios
kubectl get deployments.apps -n test-servicios
kubectl apply -f hello_clusterip.yaml -n test-servicios 
kubectl get svc -n test-servicios
# Comprobar que funciona desde dentro del cluster
kubectl run -i --tty --rm debug --image=alpine --restart=Never -n test-servicios -- wget -qO - hello-svc:80
kubectl delete service hello-svc -n test-servicios
```
> Analiza el comando que hemos ejecutado para probarlo ¿Qué hace? ¿Cómo comprueba que la conexión funciona?

### NodePort
```bash
kubectl apply -f hello_nodeport.yaml -n test-servicios
kubectl get services -n test-servicios
# Comprobar que funciona desde fuera del cluster pero en el nodo
docker exec -it minikube curl http://localhost:30100
# Otra opción de prueba utilizando minikube
minikube service hello-svc -n test-servicios
kubectl delete service hello-svc -n test-servicios
```

### LoadBalancer
```bash
kubectl apply -f hello_loadbalancer.yaml -n test-servicios
kubectl get services -n test-servicios
kubectl describe service hello-svc -n test-servicios
# Comprobar que funciona sin entrar la cluster
# Con minikube tunnel hay que fijarse en la ip externa que nos asigna en el listado de servicios
minikube tunnel
kubectl delete service hello-svc -n test-servicios
```

### :clipboard: Ejercicio 3

> Crea un deployment en un namespace llamado `clasterype` con un contenedor de [nginx](https://hub.docker.com/_/nginx) y con el parámetro `containerPort` con el valor `81`. Deberá estar configurado para tener 2 réplicas.
> Cuando tengas el deployment listo, crea un servicio de tipo `ClusterIp` que exponga el contenedor de nginx en el puerto `80`. Comprueba a ver si funciona el servicio desde dentro del cluster ¿No funciona? Intenta descubrir por qué no funciona. Haz las modificaciones necesarias utilizando el comando [`kubectl edit`](https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl_edit/)
>  
> Una vez hayas conseguido que funcione el servicio de tipo `ClusterIp` bórralo y crea un servicio de tipo `Nodeport` con los parámetros necesarios para que funcione. No le asignes un `nodePort` para que lo haga automáticamente minikube. Una vez creado, consulta qué puerto te ha asignado para hacer la prueba con el comando `docker exec`
> 
> Si ya has conseguido lo anterior. Borra el servicio de tipo `NodePort` y crea uno de tipo `LoadBalancer` que exponga el servicio en el puerto 8080. Antes de ejecutar el comando `minikube tunnel` para probarlo, lista los servicios creados y fíjate bien en los datos que aparecen. Ejecuta `minikube tunnel`, vuelve a listar los servicios. ¿Qué diferencias hay? Prueba a ver si funciona


## Almacenamiento

Recuerda que minikube es un docker, y para que pueda tener acceso dentro del contenedor a ficheros que tengamos en nuestra máquina virtual hay que montar nuestro directorio para que después se tenga acceso. Para ello ejecutar:

`minikube mount dir_maquina_virtual:dir_dentro_minikube`

### PersistemVolume y PersistenVolumeClaim

### :clipboard: Ejercicio 4

> El objetivo de este ejercicio es tener un pod de nginx levantado que muestre un index.html que podamos modificar desde nuestra máquina virtual para poder ver los cambios en el mismo momento en el que lo hagamos.
>
> Para ello tienes que crear un PersistenVolume que utilice un directorio que esté montado en minikube desde nuestra máquina virtual. Crear después un PersistenVolumeClaim y utilizarlo en un contenedor de nginx. No hace falta crear un deployment, solamente un pod.  
>
> En la [documentación de la imagen de nginx](https://hub.docker.com/_/nginx) en la sección que explica cómo utilizar la imagen tienes la información necesaria para saber "dónde" debería estar el index.html
>
> Para comprobar si funciona expón el servicio utilizando el comando [`kubectl expose`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#expose) utilizando el tipo de servicio que creas conveniente. Y después utiliza el comando `minikube service`.

## Autoescalado

Para poder utilizar el autoescalado con minikube hay que activar el addon metric-server

`minikube addons enable metrics-server`

> Para que metrics comience a funcionar hay que esperar un tiempo.

```bash
kubectl apply -f nginx_deployment.yaml
kubectl get deployments
kubectl get pods
# Podemos ver las métricas de la siguiente manera
kubectl top pod POD_NAME 
# Configurar el autoescalado
kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=1 --max=5
# Inducir carga en el pod levantado
kubectl exec -it POD_NAME -- /bin/bash
yes > /dev/null

# Para ver los valores de la monitorización
kubectl top pod POD_NAME
# Para ver si aumentan los pods
kubectl get deployments.apps -w
```
> La bajada al mínimo tardará aprox. 5 minutos. Si queremos seguir podemos forzarlo
> ejecutando `kubectl scale deployment nginx-deployment --replicas=1`

> El número de pods al que sube tiene que ver con los valores límite. La fórmula es la siguiente
> `desiredReplicas = ceil[currentReplicas * ( currentMetricValue / desiredMetricValue )]`

Si lo probamos aplicando la definición en yaml pero esta vez con el 25% veremos que escala a 4 pods

```bash
kubectl delete hpa nginx-deployment
kubectl apply -f horizontal_autoscaler.yaml
# Inducir carga en el pod levantado
kubectl exec -it POD_NAME -- /bin/bash
yes > /dev/null

kubectl get deployments.apps -w

# Limpieza
kubectl delete hpa nginx-autoscaler
kubectl delete deployment nginx-deployment 
```

## Rollback

Desplegar un nginx y modificar:
* El número de réplicas
* La memoria
* Volver a la revisión 1

```bash
# Realizar el despliegue
kubectl apply -f nginx_deployment.yaml --record
# Cambiar el número de replicas de 1 a 2
kubectl edit deployment nginx-deployment --record
kubectl annotate deployments.apps nginx-deployment kubernetes.io/change-cause="Cambio en las replicas de 1 a 2"
# Ver el historial de cambios
kubectl rollout history deployment nginx-deployment
# Ver detalle de un rollout específico
kubectl rollout history deployment nginx-deployment --revision=1
# Cambiar la memoria de 64 a 128
kubectl edit deployment nginx-deployment --record
# Volver a la versión 1
kubectl rollout undo deployment nginx-deployment --to-revision=1
```

### :clipboard: Ejercicio 5

> El objetivo de este ejercicio es tener crear un deploy con el comando `kubectl create deploy...` de una imagen llamada `ghost` activando la opción de grabar revisiones.
>
> Una vez desplegado cambiar la configuración con el comando [`kubectl set image`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-image-em-) para modificar la imagen por `ghost:09`. Anotar el cambio con el texto "Cambio de versión de la imagen". 
> Comprobar que el cambio da error, listar las revisiones disponibles y volver a la revisión anterior.
>
> Busca cómo podrías cambiar el número de revisiones que se almacenan en el historial de un despliegue (por defecto es 10).

## Supervisión

### Readiness

```bash
kubectl apply pod_readiness.yaml
kubectl get pods -w
```

### Liveness

```bash
kubectl apply pod_liveness.yaml
kubectl get pods -w
```
## Despliegues all_together

Montar el volumen de minikube

```bash
minikube mount /home/endika/data/:/data/html
```
### Several files
```bash
kubectl apply -f .
```

### One file
En este caso se crea un namespace para todo lo creado y se le pone labels

```bash
kubectl apply -f full_nginx.yaml
```

Borrar lo creado con one_file

```bash
kubectl delete all -l app=nginx -n full-nginx
```