# K8-docker-api
En este proyecto se aplica una API (node-express) y una base de datos (MySQL).
En la que se estará corriendo por K8s.

## Pasos
Primero se corre el minikube y se habilita el `registry`.

### Namespace
Se realiza los namespaces que se quieren utilizar:

```
kubectl create namespace db-k8s
kubectl create namespace api-k8s
```
### Apply
Se agregan los ```deployment``` y ```service``` tanto de la api como de la base de datos.

```
kubectl apply -f db-deployment.yml -n db-k8s
kubectl apply -f db-service.yml -n db-k8s
kubectl apply -f services.yml -n api-k8s
kubectl apply -f deployment.yml -n api-k8s
```

### Verificación
Se verifica si estan subidos correctamente los ```deployment``` y ```service```

```
kubectl get pods -A
```
### Iniciar `registry`

```
docker run --rm -it --network=host alpine ash -c "apk add socat && socat TCP-LISTEN:5000,reuseaddr,fork TCP:host.docker.internal:5000"
```

### Empujar imagen
Ahora se empuja la imagen de la API

```
docker push localhost:5000/kd-api:2
```

## Corriendo
Para saber que efectivamente la API está funcionando (y que la base de datos se está ejecutando)
se copia el nombre del pod (instancia del contenedor) que está corriendo además del namespace en el que esta sucediendo
```
kubectl logs deployment-6ff795d79f-dvlbk -n api-k8s
```

# Final
Debe de aparecer un mensaje de:

```Servidor escuchando en el puerto 3000```

```Conexión exitosa a MySOL```
