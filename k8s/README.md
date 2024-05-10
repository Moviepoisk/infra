# Устанавливаем cert-manager
helm repo add jetstack https://charts.jetstack.io --force-update
helm repo update

посетите https://cert-manager.io/docs/installation/helm/ версию поменять
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.5/cert-manager.crds.yaml

helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.14.5


# Установка Jaeger Operator

kubectl create namespace observability

создаем разрешения jaeger-operator (не помню момент когда запустил)

kubectl apply -f jaeger-operator-role.yaml
kubectl apply -f jaeger-operator-binding.yaml


kubectl create -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.57.0/jaeger-operator.yaml -n observability

kubectl get deployment jaeger-operator -n observability

Развертывание Jaeger all-in-one через Jaeger Operator
Создайте Jaeger all-in-one ресурс:
Создайте новый файл jaeger-all-in-one.yaml с таким содержимым:

apiVersion: jaegertracing.io/v1
kind: Jaeger
metadata:
  name: my-jaeger
  namespace: observability
spec:
  strategy: allInOne
  allInOne:
    options:
      log-level: debug
      memory.max-traces: 20000

Примените манифест:
Примените этот манифест к вашему Kubernetes кластеру:

kubectl apply -f jaeger-all-in-one.yaml
Проверьте развертывание:
Убедитесь, что все необходимые компоненты запущены и работают:

kubectl get pods -n observability
Получите доступ к Jaeger UI:
Используйте port-forward для доступа к UI:

kubectl port-forward svc/my-jaeger-query 16686:16686 -n observability
Теперь UI будет доступен по адресу http://localhost:16686.


# установка elastic

Установка с помощью Elastic Cloud on Kubernetes (ECK):
Установите Custom Resource Definitions (CRDs):
Примените все CRD для ECK:

kubectl apply -f https://download.elastic.co/downloads/eck/2.11.0/crds.yaml

Установите ECK Operator:
Примените оператор с помощью манифеста:

kubectl apply -f https://download.elastic.co/downloads/eck/2.11.0/operator.yaml

Проверьте состояние оператора:
Убедитесь, что оператор успешно запущен:

kubectl get pods -n elastic-system

Разверните кластер Elasticsearch:

Создайте YAML-файл elasticsearch-cluster.yaml со следующим содержимым:

apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: quickstart
  namespace: default
spec:
  version: 8.8.0
  nodeSets:
    - name: default
      count: 3
      config:
        node.store.allow_mmap: false

Примените манифест:
Примените файл к вашему кластеру:

kubectl apply -f elasticsearch-cluster.yaml

Проверьте состояние кластера:
Убедитесь, что все узлы запущены:

kubectl get pods -n default