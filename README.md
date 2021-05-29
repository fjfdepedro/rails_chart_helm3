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

## Manifiestos de configuración Kustomize y GitRepository
