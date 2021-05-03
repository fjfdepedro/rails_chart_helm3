# rails_chart_helm3

Manifiestos de Helm 3 para las aplicaciones Rails con MongoDB, Postgres y RabbitMQ
Tenemos las dos aplicaciones dockerizadas en Docker Hub:

https://hub.docker.com/repository/docker/fjfdepedro/rails_postgres

https://hub.docker.com/repository/docker/fjfdepedro/rails_mongo

Se empaqueta:
```console
#helm package ./micro-services-rails/
```
Y la instalación se realiza de la siguiente manera:
```console
#helm install --generate-name micro-services-rails-0.1.0.tgz 
```
