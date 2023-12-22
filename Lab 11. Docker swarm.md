#Практические задания

**Для выполнения практических заданий воспользовался play with docker.**


1. Выполните установку кластера с одной управляющей и двумя рабочими нодами.
2. Удалите рабочую нода из кластера и снова добавьте.
3. Создайте docker образ показывающий метаданные узла, включая ip адрес(Используйте любой язык программирования. Не забудьте открыть порт в создаваемом сервисе. EXPOSE).
4. Разверните сервис с 3 репликами (проверьте на какой ip вы попадаете).
5. Пересоберите образ указав дополнительную информацию о разработчике образа с вывод на главный интерфейс. Обновите приложение с использованием команды docker service update.

**1)** 

Для установки кластера возпользуемся в manager node следующей командой:

```sh
docker swarm init --advertise-addr 192.168.0.13
```

В ответ нам пришло:

```sh
Swarm initialized: current node (qvww0lemyrehkxiagi5cnrha4) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-0l4oc5yakp7vu87lbp7332fhuzotnxlg1qgg5rgvaonj6s33os-6dx29058plw9mn29116frp0jn 192.168.0.13:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

После чего мы оставшиеся 2 node инициализируем как worker:

```sh
docker swarm join --token SWMTKN-1-0l4oc5yakp7vu87lbp7332fhuzotnxlg1qgg5rgvaonj6s33os-6dx29058plw9mn29116frp0jn 192.168.0.13:2377
```

Получим сообщение:

```sh
This node joined a swarm as a worker.
```

**2)**

Для удаления нода из кластера, воспользуемся `docker swarm leave`

Для обратного действия, возпользуемся командой инициализации из прошлого задания

**3)**
Создадим Dockerfile который поможет получить метаданные узла, включая IP-адрес:

```
FROM alpine

RUN apk add --no-cache curl

CMD curl http://169.254.169.254/latest/meta-data/local-ipv4
```

Соберем образ:

```sh
docker build -t node-metadata .
```

Затем запустим контейнер из этого образа:

```sh
docker run --rm node-metadata
```

Это вернет IP-адрес узла, на котором запущен контейнер.

**4)**

   Для развёртывания сервиса с тремя репликами используем:

```sh
docker service create --name my-service --replicas 3 -p 80:80 nginx
```


**5)**

Для обновления образа с новой информацией о разработчике выполним следующие шаги:

1. Изменим Dockerfile, добавив новую информацию:

```
FROM nginx
LABEL maintainer="Your Name <your.email@example.com>"
```

2. Пересоберем образ:

```sh
docker build -t my-updated-image .
```

3. Обновим сервис с использованием нового образа:

```sh
docker service update --image my-updated-image my-service
```

