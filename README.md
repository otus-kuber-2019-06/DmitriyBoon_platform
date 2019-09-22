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

# homework-2

- 01
Создаем пользователей из файла
```
apiVersion: v1       # Версия api
kind: ServiceAccount # тип записи 
metadata:            # метадата
  name: bob          # имя
```
Даем права на кластер

```
apiVersion: rbac.authorization.k8s.io/v1 # Версия api
kind: ClusterRoleBinding                 # Выдача прав в кластере
metadata:  
  name: admin-01                         #  имя операции
roleRef:                                 # группы того что будем применять
  apiGroup: rbac.authorization.k8s.io    # группа api
  kind: ClusterRole                      # применяемый объект
  name: admin                            # имя события
subjects:                                # к кому будем применять
  - kind: ServiceAccount                 # к какому объекту
    name: bob                            # имя обекта
    namespace: default  

```

-  02

Создаем наймспайс, тут все просто.
```
apiVersion: v1
kind: Namespace
metadata:
  name: prometheus

```
Создаем имя в наймспасе

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: carol
  namespace: prometheus
```

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole   # выдаем роль в кластере
metadata:
  name: carol-pods  # название 
rules:
- apiGroup:
    - ""            # любая группа api 
  resources:
    - pods          # к какому обекту
  verbs:            # разрешенные действия
    - watch     
    - get 
    - list

```

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding   # выдем права в кластере
metadata:
  name: carol-bind         # имя события
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole        # Присвоение роли
  name: carol-pods         # кому эту роль назначить
subjects:                  # подписчики
- kind: Group              # выдать в группе
  name: system:serviceaccount:prometheus # в каких группах выдать роль, карол в роли промитиус получит права.
  apiGroup: rbac.authorization.k8s.io/v1
```
- 03

Созданы пользователи и наймспэсы.

Выданы права admin
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: jane-admin
  namespace: dev
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: admin
subjects:
- kind: ServiceAccount
  name: jane
  namespace: dev
```
Выданы права view
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: ken-view
  namespace: dev
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: view 
subjects:
- kind: ServiceAccount
  name: ken
  namespace: dev

```
# homework-3

 Выполнено задание без звездочки
 

# homework-4
-  Установлен minio
-  Установлен service
[использую документацию](https://medium.com/@karrier_io/minio-s3-compatible-storage-on-kubernetes-74e2cf0902f3)
### Генерируем пароли для секрета
```
date | md5sum
01f74d26e99c90da3e580e6231ad95b5  
date | md5sum
aaaf5501de32857617ebe7f707a0a75e
```

### Генерируем секреты
```
kubectl create secret generic minio-keys --from-literal=access-key=01f74d26e99c90da3e580e6231ad95b5 --from-literal=secret-key=aaaf5501de32857617ebe7f707a0a75e
```
- Редактируем statfulset

```
        env:
        - name: MINIO_ACCESS_KEY
          valueFrom:
            secretKeyRef:
              name: minio-keys
              key: access-key
        - name: MINIO_SECRET_KEY
          valueFrom:
            secretKeyRef:
              name: minio-keys
              key: secret-ke
```
- Удаляем, запускем с новыми ключами.
- env 
```
HOSTNAME=minio-0
MINIO_ACCESS_KEY_FILE=access_key
SHLVL=2
HOME=/root
MINIO_ACCESS_KEY=01f74d26e99c90da3e580e6231ad95b5
MINIO_UPDATE=off
TERM=xterm
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_PROTO=tcp
MINIO_SECRET_KEY_FILE=secret_key
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
MINIO_SECRET_KEY=aaaf5501de32857617ebe7f707a0a75e
KUBERNETES_SERVICE_HOST=10.96.0.1

```

# homework-5 SNI

- Выполенено без *
- (Использовал при выполнении)[https://kubernetes.io/blog/2018/10/09/introducing-volume-snapshot-alpha-for-kubernetes/]
- (example)[https://github.com/kubernetes-csi/external-snapshotter]