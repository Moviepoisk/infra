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