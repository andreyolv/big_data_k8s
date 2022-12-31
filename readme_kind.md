```sh
# start kind
kind create cluster --name bigdatak8s

# connect into k8s cluster
kubectx kind-bigdatak8s

# create namespaces
k create namespace cicd
k create namespace orchestrator
k create namespace database
k create namespace ingestion
k create namespace deepstorage

# k create namespace processing
# k create namespace datastore
# k create namespace tracing
# k create namespace logging
# k create namespace monitoring
# k create namespace viz
# k create namespace app
# k create namespace cost
# k create namespace misc
# k create namespace dataops
# k create namespace gateway

# add & update helm list repos
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

# install crd's [custom resources]
# argo-cd
# https://artifacthub.io/packages/helm/argo/argo-cd
# https://github.com/argoproj/argo-helm
helm install argocd argo/argo-cd --namespace cicd --version 3.26.8

# install argo-cd [gitops]
# create a load balancer
k patch svc argocd-server -n cicd -p '{"spec": {"type": "LoadBalancer"}}'
k patch svc argocd-server -n cicd -p '{"spec": {"type": "LoadBalancer", "externalIPs":["172.31.71.218"]}}'

# other terminal run port forward
kubectl port-forward service/argocd-server -n cicd 8080:443

# create cluster role binding for admin user [sa]
k create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=system:serviceaccount:cicd:argocd-application-controller -n cicd

# register cluster
CLUSTER="kind-bigdatak8s"
argocd cluster add $CLUSTER --in-cluster

# add repo into argo-cd by UI

```
kind delete cluster bigdatak8s
```sh
# helm
helm repo add strimzi https://strimzi.io/charts/
helm repo add spark-operator https://googlecloudplatform.github.io/spark-on-k8s-operator
helm repo add apache-airflow https://airflow.apache.org/
helm repo update

# strimzi
helm install kafka strimzi/strimzi-kafka-operator --namespace ingestion --version 0.31.1

# spark
helm install spark spark-operator/spark-operator --namespace processing --set image.tag=v1beta2-1.3.0-3.1.1

# config maps
k apply -f repository/yamls/ingestion/metrics/kafka-metrics-config.yaml
k apply -f repository/yamls/ingestion/metrics/zookeeper-metrics-config.yaml
k apply -f repository/yamls/ingestion/metrics/connect-metrics-config.yaml
k apply -f repository/yamls/ingestion/metrics/cruise-control-metrics-config.yaml

# ingestion
k apply -f repository/app-manifests/ingestion/kafka-broker-ephemeral.yaml
k apply -f repository/app-manifests/ingestion/schema-registry.yaml
k apply -f repository/app-manifests/ingestion/kafka-connect.yaml
k apply -f repository/app-manifests/ingestion/cruise-control.yaml
k apply -f repository/app-manifests/ingestion/kafka-connectors.yaml

# databases
k apply -f repository/app-manifests/database/mssql.yaml
k apply -f repository/app-manifests/database/mysql.yaml
k apply -f repository/app-manifests/database/postgres.yaml
k apply -f repository/app-manifests/database/mongodb.yaml
k apply -f repository/app-manifests/database/yugabytedb.yaml

# deep storage
k apply -f repository/app-manifests/deepstorage/minio-operator.yaml
k apply -f repository/app-manifests/deepstorage/minio.yaml

# datastore
k apply -f repository/app-manifests/datastore/pinot.yaml

# processing
k apply -f repository/app-manifests/processing/ksqldb.yaml
k apply -f repository/app-manifests/processing/trino.yaml

# orchestrator
k apply -f repository/app-manifests/orchestrator/airflow.yaml

# data ops
k apply -f repository/app-manifests/lenses/lenses.yaml

# monitoring
k apply -f repository/app-manifests/monitoring/prometheus-alertmanager-grafana-botkube.yaml

# logging
k apply -f repository/app-manifests/logging/elasticsearch.yaml
k apply -f repository/app-manifests/logging/filebeat.yaml
k apply -f repository/app-manifests/logging/kibana.yaml

# cost
k apply -f repository/app-manifests/cost/kubecost.yaml

# load balancer
k apply -f repository/app-manifests/misc/load-balancers-svc.yaml

# deployed apps
k get applications -n cicd

# housekeeping
helm delete argocd -n cicd
helm delete kafka -n ingestion
helm delete spark -n processing
```
