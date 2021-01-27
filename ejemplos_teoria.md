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

### NodePort
```bash
kubectl apply -f hello_nodeport.yaml -n test-servicios
kubectl get services -n test-servicios
# Comprobar que funciona desde fuera del cluster pero en el nodo
docker exec -it minikube curl http://localhost:30100
kubectl delete service hello-svc -n test-servicios
```

### LoadBalancer
```bash
kubectl apply -f hello_loadbalancer.yaml -n test-servicios
kubectl get services -n test-servicios
kubectl describe service hello-svc -n test-servicios
# Comprobar que funciona sin entrar la cluster
minikube service hello-svc -n test-servicios
kubectl delete service hello-svc -n test-servicios
```

## Almacenamiento

Para poder probar la creación de volúmenes lo que vamos a tener que hacer es crear un directorio que se llame html y crear dentro un index.html con un texto de prueba
```bash
mkdir /home/endika/data
cd /home/endika/data
echo "<html><body>Mi html de prueba</body></html>" > index.html
```
Puesto que minikube es un docker, dentro hay que montar nuestro directorio para que después se tenga acceso. Para ello ejecutar:
`minikube mount /home/endika/data/:/data/html`

### PersistemVolume y PersistenVolumeClaim
```bash
kubectl apply -f persistentvolume.yaml
kubectl get pv
kubectl apply -f persistentvolumeclaim.yaml
kubectl get pvc
kubectl apply -f podwithvolume.yaml
kubectl expose pod task-pv-pod --type=LoadBalancer --port=80
minikube service task-pv-pod
```

Probar a cambiar el html y ver que se ven los cambios.

```bash
# Limpieza
kubectl delete pod task-pv-pod
kubectl delete pvc task-pv-claim
kubectl delete pv task-pv-volume
kubectl delete service task-pv-pod
```

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