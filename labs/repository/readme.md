```sh
# connect into k8s cluster
kubectx do-nyc1-k8s-training-labs

# create namespaces
k create namespace orchestrator
k create namespace database
k create namespace ingestion
k create namespace processing
k create namespace datastore
k create namespace deepstorage
k create namespace app

```

```sh

# helm
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add yugabyte https://charts.yugabyte.com
helm repo add elastic https://helm.elastic.co
helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com
helm repo add strimzi https://strimzi.io/charts/
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
helm repo add apache-airflow https://airflow.apache.org
helm repo add valeriano-manassero https://valeriano-manassero.github.io/helm-charts
helm repo add spark-operator https://googlecloudplatform.github.io/spark-on-k8s-operator
helm repo add minio https://charts.min.io/
helm repo update

# strimzi
helm install kafka strimzi/strimzi-kafka-operator --namespace ingestion --version 0.26.0

# spark
helm install spark spark-operator/spark-operator --namespace processing --set image.tag=v1beta2-1.3.0-3.1.1

# ingestion
k apply -f repository/yamls/ingestion/broker/kafka-ephemeral.yaml -n ingestion
k apply -f repository/yamls/ingestion/broker/kafka-jbod.yaml -n ingestion
k apply -f repository/yamls/ingestion/kafka-connect/kafka-connect.yaml -n ingestion
helm install schema-registry repository/helm-charts/cp-schema-registry -n ingestion


# databases
helm install yugabytedb yugabyte/yugabyte -f repository/helm-charts/yugabyte/values.yaml -n database
helm install postgres bitnami/postgresql -f repository/helm-charts/postgres/values-development.yaml -n database
helm install mssql stable/mssql-linux -f repository/helm-charts/mssql/values-development.yaml -n database
helm install mongodb bitnami/mongodb-sharded -f repository/helm-charts/mongodb/values.yaml -n database
helm install mysql bitnami/mysql -f repository/helm-charts/mysql/values-development.yaml -n database

# deepstorage
helm install minio repository/helm-charts/minio/ -n deepstorage

# datastore
helm install pinot repository/helm-charts/pinot -n datastore

# processing
k apply -f repository/yamls/processing/ksqldb/ -n processing
helm install trino valeriano-manassero/trino -f repository/helm-charts/trino/values-development.yaml -n processing

# orchestrator
helm install airflow apache-airflow/airflow -f repository/helm-charts/airflow/values-development.yaml  -n orchestrator

# lb

k apply -f repository/svc-lbs/svc-lb-airflow-ui.yaml -n orchestrator

# housekeeping
```
