# Практические задания

1. Примонтируйте volume в проект из предыдущей работы.
2. Повторите примеры для СУБД PostgeSQL, сгенерировав несколько произвольных таблиц.
3. Проинспектируйте docker контейнер из примера 12, на наличие volume, запустите.
4. Выполните пример 6 для nginx, предварительно узнав порт, скопируйте внутрь образа конфигурационный файл, а также измените стартовую страницу. Проверьте работоспособность используя команду curl.

**1)**
Для начала создадим Volume nuxt-volume:

```sh
sudo docker volume create my-nuxt-volume
```

После чего примонтируем его в проект из Лабораторной работы №4 с помощью флага `-volume(-v)`:

```sh
sudo docker run -itd -p 8002:80 --name testnuxtdeploy -v nuxt-volume:/public testnuxtdeploy
```

При успешном выполнении команды получим ответ от консоли с id контейнера:

```sh
sudo docker run -itd -p 8002:80 --name testnuxtdeploy -v nuxt-volume:/public testnuxtdeploy
8a730bb2927819f0d5df88a4cee2d644e03561133669e3460e770630a51aff0b
```

После инспектирования контейнера testnuxtdeploy в графе Mounts увидем успешное добавление Volume:

```sh
"Mounts": [
            {
                "Type": "volume",
                "Name": "nuxt-volume",
                "Source": "/var/lib/docker/volumes/nuxt-volume/_data",
                "Destination": "/public",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],

```

**2)**

Для начала создадим контейнер PostgreSQL:

```sh
sudo docker run -d --name postgres-container -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 postgres:latest
```

После чего подключимся к контейнеру и создадим в нем пару таблиц:

```sh
sudo docker exec -it postgres-container psql -U postgres
```

Для добавления таблиц будем использовать стандартные SQL запросы:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    age INT
);

INSERT INTO users (name, age) VALUES ('Кирилл', 17), ('Кристина', 20), ('Хулио', 35);

CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    price NUMERIC(10, 2)
);

INSERT INTO products (product_name, price) VALUES ('Яблоки', 49.99), ('Тыблоки', 29.99);
```

Для того, что бы проверить выполнение команды выведем значения таблиц:

``` sql
postgres=# SELECT * FROM users;
 id |   name   | age 
----+----------+-----
  7 | Кирилл   |  17
  8 | Кристина |  20
  9 | Хулио    |  35
(3 rows)

postgres=# SELECT * FROM products;
 id | product_name | price 
----+--------------+-------
  5 | Яблоки       | 49.99
  6 | Тыблоки      | 29.99
(2 rows)
```

Создадим volume:

```sh
sudo docker volume create postgres-volume
```

Подключим к контейнеру:

```sh
sudo docker run -d --name postgres-container -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -v postgres-volume:/var/lib/postgresql/data postgres:latest
```

**3)**

```sh
"Mounts": [
            {
                "Type": "tmpfs",
                "Source": "",
                "Destination": "/app",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

**4)**

Создадим контейнер Nginx:

```sh
sudo docker run --name temp-nginx -d nginx:latest
```

Создадим index.html и nginx.conf со следующими кодами:

index.html:
```
<!-- index.html -->

<!DOCTYPE html>
<html>
<head>
    <title>Моя тестовая страница</title>
</head>
<body>
    <h1>Добро пожаловать на мою тестовую страницу!</h1>
    <p>Это простая тестовая страница для проверки работоспособности Nginx в контейнере Docker.</p>
</body>
</html>
```

nginx.conf:
```
# nginx.conf

# Основные настройки
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /var/run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;

    server {
        listen 80;
        server_name localhost;

        location / {
            root /usr/share/nginx/html;
            index index.html;
        }
    }
}
```

После чего перенесем необходимые файлы для сохранения в Volume:

```sh
sudo docker cp nginx.conf temp-nginx:/etc/nginx/nginx.conf
```

```
sudo docker cp index.html temp-nginx:/usr/share/nginx/html/index.html
```

Создаем Volume:

```sh
sudo docker volume create nginx-config
```

После чего создаем сервер nginx, в котором будет использоваться наш настроенный Volume с необходимыми файлами:

```sh
sudo docker container run -d -p 8080:80 -v nginx-config:/etc/nginx -v temp-nginx:/usr/share/nginx/html --name my-nginx nginx:latest
```

После чего проверим все через curl:

```sh
curl localhost:8080
<!-- index.html -->

<!DOCTYPE html>
<html>
<head>
    <title>Моя тестовая страница</title>
</head>
<body>
    <h1>Добро пожаловать на мою тестовую страницу!</h1>
    <p>Это простая тестовая страница для проверки работоспособности Nginx в контейнере Docker.</p>
</body>
</html>
```
