# DmitriyBoon_platform
DmitriyBoon Platform repository

- homework - 1
Установлены: minikube, kind, kubectl, dashboard.
 - Чтобы поставить дашбоард, уставновил clusterRole
 ```
 apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
```
- И добавил пользователя в namespace
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
```
Сгенерировал token

Сделан Dockerfile

Описан web-pod.yaml

