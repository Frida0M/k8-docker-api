# K8-docker-api
En este proyecto se aplica una API (node-express) y una base de datos (MySQL).
En la que se estará corriendo por K8s.

## Pasos
Primero se corre el minikube y se realiza el `registry`.

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
