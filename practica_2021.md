#Enunciado de la práctica

Arrancar los contenedores creados en la anterior parte del laboratorio en pods separados de kubernetes en un namespace llamado miku:

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
* Control para que el pod de la aplicación no arranque si no existe el pod de MySQL.