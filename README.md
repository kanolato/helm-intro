# helm-intro
## Prerrequisitos
1. [Minikube](https://github.com/kubernetes/minikube#installation)
2. [Helm](https://docs.helm.sh/using_helm/#installing-helm)
3. [Helm registry plugin](https://github.com/app-registry/appr-helm-plugin)
## Inicializando minikube.
Minikube es una herramienta que nos permite tener un cluster de kubernetes de un nodo de manera local, perfecto para hecharle un ojo a kubernetes o para desarrollar en el dia a dia.

Para inicializar minikube y asignarle 4086mb de memoria RAM, ejecutamos:
```
$ minikube start --memory 4086
```
El comando de arriba, crea la configuracion necesaria para comunicarnos con el cluster de minikube, tambien pone esta configuracion como default, para asi poder utilizar de inmediato el cluster.

## Inicializacion de Helm.
Se inicializara `helm tiller` sin hacer uso de [RBAC]. (https://kubernetes.io/docs/reference/access-authn-authz/rbac/).
```
$ helm init
```
## Install, Update & Delete Charts.
1. Instalando PostgreSQL y RabbitMQ
```
# POSTGRESQL
$ helm install --namespace data --name postgres \
	--set imageTag=9.6.2,postgresUser=postgres,postgresPassword=stUD0EtyJQ,postgresDatabase=warehouse \
	stable/postgresql
# RABBITMQ
$ helm install --namespace data --name rabbit \
	--set imageTag=3.7.3-r0,rabbitmqUsername=user,rabbitmqPassword=e8TAJzS4im,rabbitmq.username=user,rabbitmq.password=e8TAJzS4im \
	stable/rabbitmq --version 0.6.18
```
2. Instalando warehouse
```
$ helm registry install quay.io/kanolato/warehouse \
    --name my-warehouse \
    --set minikube=true
```

3. Actualizando frontend de warehouse
```
$ helm registry upgrade quay.io/kanolato/warehouse \
    my-warehouse \
    --set frontend.image=circulo7/cursos-k8s-admin-frontend:grpc-0.0.4,minikube=true
```

4. Eliminando todo
```
$ helm delete my-warehouse --purge
$ helm delete postgres --purge
$ helm rabbit postgres --purge
```


