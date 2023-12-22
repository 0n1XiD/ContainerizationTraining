# Практические задания


1. Создание Pod:

* Создайте YAML-файл для Pod с именем "my-pod.yaml".
* Определите метаданные и спецификацию Pod.
* Запустите Pod с помощью команды kubectl apply -f my-pod.yaml.
* Проверьте статус запущенного Pod с помощью команды kubectl get pods.



2. Масштабирование ReplicaSet:

* Создайте YAML-файл для ReplicaSet с именем "my-replicaset.yaml".
* Определите метаданные, спецификацию ReplicaSet и шаблон Pod.
* Запустите ReplicaSet с помощью команды kubectl apply -f my-replicaset.yaml.
* Проверьте количество запущенных подов с помощью команды kubectl get pods.
* Измените количество реплик в ReplicaSet с помощью команды kubectl scale replicaset my-replicaset --replicas=5.
* Убедитесь, что количество запущенных подов увеличилось до 5.



3. Развёртывание приложения с использованием Deployment:

* Создайте YAML-файл для Deployment с именем "my-deployment.yaml".
* Определите метаданные, спецификацию Deployment и шаблон Pod.
* Запустите Deployment с помощью команды kubectl apply -f my-deployment.yaml.
* Проверьте количество запущенных подов с помощью команды kubectl get pods.
* Обновите образ контейнера в Deployment с помощью команды kubectl set image deployment/my-deployment my-container=my-new-image:latest.
* Убедитесь, что обновление образа контейнера прошло успешно и новые поды с новым образом были развернуты.



4. Удаление объектов:

* Удалите Pod с помощью команды kubectl delete pod my-pod.
* Удалите ReplicaSet с помощью команды kubectl delete replicaset my-replicaset.
* Удалите Deployment с помощью команды kubectl delete deployment my-deployment.

**1)**

* С помощью nano создали `my-pod.yaml` со следующим кодом:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx:latest
```

* Запускаем Pod:

```sh
kubectl apply -f my-pod.yaml
pod/my-pod created
```

*Проверяем статус Pod:

```sh
kubectl get pods

NAME     READY   STATUS    RESTARTS   AGE
my-pod   1/1     Running   0          52s

```

**2)**

* С помощью nano создали `my-replicaset.yaml` со следующим кодом:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
```

* Запускаем ReplicaSet:
```sh
kubectl apply -f my-replicaset.yaml
replicaset.apps/my-replicaset created
```

* Проверяем количество Pod:
```sh
kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
my-pod                1/1     Running   0          14m
my-replicaset-f68xn   1/1     Running   0          2m3s
my-replicaset-ssplv   1/1     Running   0          2m3s
my-replicaset-tjv5r   1/1     Running   0          2m3s
```

* Увеличиваем количество реплик в ReplicaSet до 5:
```sh
kubectl scale replicaset my-replicaset --replicas=5
replicaset.apps/my-replicaset scaled
```

* Проверяем количество Pod:
```sh
kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
my-pod                1/1     Running   0          15m
my-replicaset-f68xn   1/1     Running   0          3m12s
my-replicaset-ssplv   1/1     Running   0          3m12s
my-replicaset-tjv5r   1/1     Running   0          3m12s
my-replicaset-tz97d   1/1     Running   0          21s
my-replicaset-vr9vh   1/1     Running   0          21s
```

**3)**

* С помощью nano создали `my-deployment.yaml` со следующим кодом:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
```

* Запускаем Deployment:
```sh
kubectl apply -f my-deployment.yaml
deployment.apps/my-deployment created
```

* Проверяем количество Pod:
```sh
kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
my-deployment-584545b6d-4fq9p   1/1     Running   0          16s
my-deployment-584545b6d-dll9p   1/1     Running   0          16s
my-deployment-584545b6d-gkzph   1/1     Running   0          16s
my-pod                          1/1     Running   0          15m
my-replicaset-f68xn             1/1     Running   0          3m48s
my-replicaset-ssplv             1/1     Running   0          3m48s
my-replicaset-tjv5r             1/1     Running   0          3m48s
my-replicaset-tz97d             1/1     Running   0          57s
my-replicaset-vr9vh             1/1     Running   0          57s

```

* Обновляем образ контейнера в Deployment:
```sh
kubectl set image deployment/my-deployment my-container=my-new-image:latest
deployment.apps/my-deployment image updated
```

* Проверяем количество Pod:
```sh
kubectl get pods
NAME                             READY   STATUS         RESTARTS   AGE
my-deployment-584545b6d-4fq9p    1/1     Running        0          87s
my-deployment-584545b6d-dll9p    1/1     Running        0          87s
my-deployment-584545b6d-gkzph    1/1     Running        0          87s
my-deployment-6bf4b8898f-bs98c   0/1     ErrImagePull   0          9s
my-pod                           1/1     Running        0          17m
my-replicaset-f68xn              1/1     Running        0          4m59s
my-replicaset-ssplv              1/1     Running        0          4m59s
my-replicaset-tjv5r              1/1     Running        0          4m59s
my-replicaset-tz97d              1/1     Running        0          2m8s
my-replicaset-vr9vh              1/1     Running        0          2m8s
```

**4)**

Удаляем все созданные объекты:

```sh
onix@onix-VirtualBox:~/Рабочий стол/DevopsLabs/k8s$ kubectl delete pod my-pod
pod "my-pod" deleted
onix@onix-VirtualBox:~/Рабочий стол/DevopsLabs/k8s$ kubectl delete replicaset my-replicaset
replicaset.apps "my-replicaset" deleted
onix@onix-VirtualBox:~/Рабочий стол/DevopsLabs/k8s$ kubectl delete deployment my-deployment
deployment.apps "my-deployment" deleted
```
