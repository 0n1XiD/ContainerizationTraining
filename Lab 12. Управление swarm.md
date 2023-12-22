# Практические задания

1. Создайте собственный стек, подобный примеру.
2. Установите для каждого из сервиса на стеке несколько реплик и все их разместите на мастер ноде.
3. Определите количество оперативной памяти на мастер ноде и воркер нодах.
4. При помощи параметра placement разместите новый сервис на ноде с потреблением меньшего количества ресурсов.
5. Установите зарезервированное и максимальное количество ресурсов для сервиса, выведите результаты проинспектировав сервис после деплоя.
6. Создайте секрет и добавьте его к котейнеру с базой данных.


**1-2)**

Для этого создадим файл `my-stack.yml` с подобным содержимым:

```yaml
   version: "3.9"

   services:
     nginx:
       image: nginx:alpine
       deploy:
         replicas: 3
         placement:
           constraints:
             - node.role == manager
     whoami:
       image: containous/whoami
       deploy:
         replicas: 3
         placement:
           constraints:
             - node.role == manager
```

В этом файле мы создаем два сервиса (`nginx` и `whoami`), каждый из которых размещается на управляющей ноде (мастер-ноде) с помощью параметра `placement`.

**3)**

Для получения информации о ресурсах на нодах можно использовать команду:

```sh
docker node inspect node1 - node3
```

**4)**

Для размещения сервиса на ноде с меньшим потреблением можно использовать параметр `--constraint` с командой `docker service create`:

```sh
docker service create --name my-service --constraint 'node.role == worker'
```

Это разместит сервис `my-service` на воркер-ноде

**5)**

Для установки ограничений на ресурсы сервиса можно использовать параметры `--reserve-memory` и `--limit-memory` во время создания сервиса:

```sh
docker service create --name my-service --reserve-memory 100m --limit-memory 500m
```

Это установит зарезервированный объем памяти в 100 МБ и максимальный объем памяти в 500 МБ для сервиса `my-service`

**6)**

Для создания секрета используем команду `docker secret create`:

```bash
echo "mysecretdata" | docker secret create mysecretdata -
```

```bash
docker service create --name my-db --secret mysecretdata
```

Это создаст сервис `my-db`, использующий секрет `mysecretdata`.
