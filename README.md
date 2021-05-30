# Manifiestos de Helm 3 para los microservicios

Manifiestos de Helm 3 para las aplicaciones Rails con MongoDB, Postgres y RabbitMQ
Tenemos las dos aplicaciones dockerizadas en Docker Hub:

https://hub.docker.com/repository/docker/fjfdepedro/rails_postgres

https://hub.docker.com/repository/docker/fjfdepedro/rails_mongo

Se empaqueta:
```sh
$ helm package ./micro-services-rails/
```
Y la instalación se realiza de la siguiente manera:
```sh
$ helm install --generate-name micro-services-rails-0.1.0.tgz 
```

## Instalación de Flux

Siguiendo la documentacion de FluxCD, primero nos bajamos el comando Flux y luego con 'flux bootstrap' vamos a asociar nuestro repositorio

```
flux bootstrap github \
  --owner=fjfdepedro \
  --repository=rails_chart_helm3 \
  --branch=main \
  --path=./clusters/my-cluster \
  --personal
```

## Instalación de Flagger con Istio

Seguimos la documentación de instalación de Flagger con Istio: https://docs.flagger.app/install/flagger-install-on-kubernetes

Añadir Flagger Helm repository
```
helm repo add flagger https://flagger.app
```
Instalar Canary custom resource de Flagger, que es a partir del cual podremos crear nuestor Canary resources:
```
kubectl apply -f https://raw.githubusercontent.com/fluxcd/flagger/main/artifacts/flagger/crd.yaml
```
Crear el deployment de Flagger con Istio:
```
helm upgrade -i flagger flagger/flagger \
--namespace=istio-system \
--set crd.create=false \
--set meshProvider=istio \
--set metricsServer=http://prometheus:9090
```


## Manifiestos de configuración Kustomize y GitRepository

GitRepository:
```
apiVersion: source.toolkit.fluxcd.io/v1beta1
kind: GitRepository
metadata:
  name: rails-chart-helm3-repo
  namespace: prod
spec:
  interval: 5m
  url: https://github.com/fjfdepedro/rails_chart_helm3
  ref:
    branch: main
```
Kustomize:
```
apiVersion: kustomize.toolkit.fluxcd.io/v1beta1
kind: Kustomization
metadata:
  name: rails-chart-helm3
  namespace: prod
spec:
  interval: 15m
  path: "./apps"
  prune: true
  sourceRef:
    kind: GitRepository
    name: rails-chart-helm3-repo
```
