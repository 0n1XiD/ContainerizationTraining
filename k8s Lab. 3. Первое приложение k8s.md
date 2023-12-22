# Практические задания


1. Создание ClusterIP сервиса:

Создайте манифест для пода, который будет представлять ваше приложение.
Создайте манифест для ClusterIP сервиса, который будет пробрасывать трафик на под вашего приложения.
Примените оба манифеста в вашем кластере Kubernetes.
Убедитесь, что сервис и под работают корректно, путем проверки доступности сервиса из других подов в кластере.


2. Создание NodePort сервиса:

Создайте манифест для пода, который будет представлять ваше приложение.
Создайте манифест для NodePort сервиса, который будет пробрасывать трафик на под вашего приложения через открытый порт на каждом узле кластера.
Примените оба манифеста в вашем кластере Kubernetes.
Убедитесь, что сервис и под работают корректно, путем проверки доступности сервиса с помощью IP-адреса и порта узла кластера.


3. Создание LoadBalancer сервиса:

Создайте манифест для пода, который будет представлять ваше приложение.
Создайте манифест для LoadBalancer сервиса, который будет автоматически создавать и настраивать балансировщик нагрузки в вашей облачной среде.
Примените оба манифеста в вашем кластере Kubernetes.
Убедитесь, что сервис и под работают корректно, путем проверки доступности сервиса через внешний IP-адрес балансировщика нагрузки.


4. Создание ExternalName сервиса:

Создайте манифест для ExternalName сервиса, который будет проксировать DNS-имя к внешнему ресурсу из вашего кластера.
Примените манифест в вашем кластере Kubernetes.
Убедитесь, что сервис работает корректно, путем проверки доступности внешнего ресурса через DNS-имя сервиса.

**1)**

* Создадим файл `pod.yaml` с описанием Pod:
    
```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: my-app-pod
    spec:
      containers:
      - name: my-app-container
        image: nginx:latest
```

* Создадим файл `clusterip-service.yaml` с описанием ClusterIP сервиса:

```yaml
apiVersion: v1
kind: Service
metadata:
 name: my-clusterip-service
spec:
 selector:
   app: my-app
ports:
- protocol: TCP
port: 80
targetPort: 80
type: ClusterIP
```


* Применение манифестов:

```sh
kubectl apply -f pod.yaml
kubectl apply -f clusterip-service.yaml
```


**2)**

* Будем использовать тот же файл `pod.yaml`, который мы создали для предыдущего задания.

* Создадим файл `nodeport-service.yaml` с описанием NodePort сервиса:

```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: my-nodeport-service
    spec:
      selector:
        app: my-app
      ports:
        - protocol: TCP
          port: 80
          targetPort: 80
      type: NodePort
```

* Применение манифестов:

```sh
kubectl apply -f nodeport-service.yaml
```


**3)**

* Будем использовать тот же файл `pod.yaml`, который мы создали для предыдущего задания.

* Создадим файл `loadbalancer-service.yaml` с описанием LoadBalancer сервиса:

```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: my-loadbalancer-service
    spec:
      selector:
        app: my-app
      ports:
        - protocol: TCP
          port: 80
          targetPort: 80
      type: LoadBalancer
```

* Применение манифестов:

```sh
kubectl apply -f loadbalancer-service.yaml
```

**4)**

* Создадим файл `externalname-service.yaml` с описанием ExternalName сервиса:

```yaml
    kind: Service
    apiVersion: v1
    metadata:
      name: my-externalname-service
    spec:
      type: ExternalName
      externalName: external-service.example.com
```


* Применение манифеста:

```sh
kubectl apply -f externalname-service.yaml
```

