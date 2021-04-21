# Práctica kubernetes

## Enunciado

Para la realización de esta práctica se solicita la creación de un cluster de minikube con un registry configurado. En este cluster habrá que arrancar los contenedores creados en la anterior parte del laboratorio en pods separados de kubernetes en un namespace llamado miku:

1. El pod de base de datos tendrá que:
    * tener un volúmen donde poder persistir los datos
        * El volúmen tiene que montarse en el directorio /data de minikube
    * Configurarse con solo una réplica
    * Límite de CPU: 0.5
    * Límite memoria: 800 Mi

2. El pod de Node.js tendrá las siguientes características:
    * El número de pods en un inicio tendrá que ser 1
    * Límite de CPU: 0.3
    * Límite memoria: 200 Mi

3. Se tiene que acceder a la aplicación mediante un servicio de tipo _LoadBalancer_

## Extras

1. Despliegue de un cluster con 2 workers
2. Configuración del escalado automático 
3. Configuración de liveness y readiness
4. Añadir algún componente de kubernetes extra

> **NOTA:** Si quieres hacer la parte **extra** de la práctica aquí tienes algunas configuraciones a tener en cuenta. 

### Pod de Mysql
* Configurar prueba de **readiness** para comprobar el puerto en el que levante. La configuración de esta prueba tiene que:
    * empezar con un delay de 20 segundos
    * comprobarse cada 60 segundos

### Pod Node.js
* Configurar el **autoescalado** para que cuando se supere el 50% de uso de cpu se escale automáticamente hasta un límite de 2
* Configurar un test de **liveness** de tipo http que:
    * empieze con un delay de 40 segundos
    * se compruebe cada 70 segundos
    * no se reinicie hasta el segundo chequeo

Hay algunas mejoras que se pueden incluir. Te proponemos alguna por si quisieras investigar:
* Inicialización de la base de datos.
* Variables mediante configmaps/secrets.
* Control para que el pod de la aplicación no arranque si no existe el pod de MySQL.

## Entregables

1. Entregar en un paquete tar comprimido con tu nombre y apellido (p.e. NombreApellido.tar.gz) que contenga:
   * los yaml de definición de todos los objetos pedidos en el enunciado. 
   * un fichero grabado con `asciinema` en el que se vea cómo se consulta lo siguiente:
     * listado de servicios
     * listado de deployments
     * listado de persistent volume y persistent volume claim
     * listado de pods
     * descripción del pod de base de datos
     * descripción del deployment de la aplicación 

2. En caso de creerlo conveniente, añadir un fichero README.txt con lo que se quiera comentar: comandos de despliegue, configuraciones a tener en cuenta, problemas encontrados y soluciones, etc.

## Evaluación

|||
|------|-------:|
| Arquitectura solicitada | 50% |
| Despliegue 2 workers | +5% |
| Escalado automático | +15% |
| Liveness & Readiness| +20% |
| Componentes extra | +10% |