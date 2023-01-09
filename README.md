## Emoji Inclusion Demo App for TAP

Demo App for TAP


## Tanzu Application Platform Accelerator

To add the accelerator in Tanzu Application Platform

~~~
tanzu acc create inclusion --git-repo https://github.com/fklein82/emoji-inclusion --git-branch main --interval 5s\n
~~~

## Service Binding with PostgreSQL

~~~
```
NAMESPACE=dev

helm repo add bitnami https://charts.bitnami.com/bitnami

helm install emoji oci://harbor.emea.end2end.link/vac-global-library/charts/centos-7/postgresql --set auth.database=emoji --set global.postgresql.auth.existingSecret=db-binding-compatible -n dev

helm install emoji bitnami/postgresql --set global.postgresql.auth.postgresPassword=password \
  --set global.postgresql.auth.database=emoji -n $NAMESPACE

kubectl create secret generic db-binding-compatible --from-literal=type=postgresql --from-literal=provider=bitnami \
 --from-literal=jdbc-url=jdbc:postgresql://emoji-postgresql.${NAMESPACE}.svc.cluster.local:5432/emoji \
 --from-literal=password=password --from-literal=username=postgres -n $NAMESPACE
 
 tanzu service claim create db-binding-compatible \
  --resource-name db-binding-compatible \
  --resource-kind Secret \
  --resource-api-version v1
 
 kubectl apply -f config/workload.yaml -n $NAMESPACE
```
~~~