#Практические задания

1. Остановите все запущенные контейнеры, кроме portainer
Команда останавливающая все контейнеры docker stop $(docker ps -a -q)
Параметр -q выводит только идентификаторы контейнеров
Удалите все контейнеры docker container rm $(docker ps -a -q)
Удалите все образы docker image prune -a
Удалите все диски docker volume rm $(docker volume ls -q)
Чтобы удалить все остановленные контейнеры и неиспользуемые образы:
 docker system prune -a


2. Установите докер registry от данного автора (https://hub.docker.com/r/jc21/registry-ui)


3. Установите Shipyard и подключите к API docker.


4. Установите лимит на контейнер apache.

**1)**
После удаления всех контейнеров и образов проверим их наличие:

```sh
sudo docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

```sh
sudo docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

**2)**

Создаем через nano - docker-compose.yml в него загоняем следующий код:

```
version: "2"
services:
  registry:
    image: registry:2
    environment:
      - REGISTRY_HTTP_SECRET=o43g2kjgn2iuhv2k4jn2f23f290qfghsdg
      - REGISTRY_STORAGE_DELETE_ENABLED=
    volumes:
      - ./registry-data:/var/lib/registry
  ui:
    image: jc21/registry-ui
    environment:
      - NODE_ENV=production
      - REGISTRY_HOST=registry:5000
      - REGISTRY_SSL=
      - REGISTRY_DOMAIN=
      - REGISTRY_STORAGE_DELETE_ENABLED=
    links:
      - registry
    restart: on-failure
  proxy:
    image: jc21/registry-ui-proxy
    ports:
      - 80:80
    depends_on:
      - ui
      - registry
    links:
      - ui
      - registry
    restart: on-failure
```

Запускаем скрипт с помощью:

```sh
sudo docker-compose up -d
```

Получаем следующий ответ:


```sh
Creating network "onix_default" with the default driver
Pulling registry (registry:2)...
2: Pulling from library/registry
c926b61bad3b: Pull complete
5501dced60f8: Pull complete
e875fe5e6b9c: Pull complete
21f4bf2f86f9: Pull complete
98513cca25bb: Pull complete
Digest: sha256:0a182cb82c93939407967d6d71d6caf11dcef0e5689c6afe2d60518e3b34ab86
Status: Downloaded newer image for registry:2
Pulling proxy (jc21/registry-ui-proxy:)...
latest: Pulling from jc21/registry-ui-proxy
68ced04f60ab: Pull complete
c4039fd85dcc: Pull complete
c16ce02d3d61: Pull complete
bc9c5f04f14c: Pull complete
82ac3c2452c6: Pull complete
11afae49c216: Pull complete
Digest: sha256:b39343577eced0c6607688181c442daa483c1d47f381e454467ea70022770e6b
Status: Downloaded newer image for jc21/registry-ui-proxy:latest
Creating onix_registry_1 ... done
Creating onix_ui_1       ... done
Creating onix_proxy_1    ... done

```

**3)**

Изменяем файл Docker-compose.yml, добавив следующий сервис:

```
  shipyard:
    image: shipyard/shipyard:latest
    ports:
      - "8080:8080"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    command: -c /shipyard/shipyard.conf
```

Запускаем скрипт так же как и в задании 2

**4)**

Используем команду

```sh
sudo docker run -d -p 8083:80 -m 50m --cpus 0.01 --name ng_limit httpd
```
