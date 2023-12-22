## Т.к. в лабораторной нет точного описания практического задания, сделал примеры резервного копирования

Для выполнения бэкапа Docker-контейнеров и данных можно использовать инструменты, такие как `docker export`, `docker save` для образов и `docker volume backup` для томов данных.

1. **Бэкап Docker-контейнера:**

```sh
# Получение ID запущенного контейнера
docker ps

# Создание архива с данными контейнера
sudo docker export -o /container_backup.tar b27465272ed375ff93e373fe7039b85f04e1b1192c3048296eb6ebde1663d38e
```

2. **Бэкап Docker-образа:**

```sh
# Получение списка образов
docker images

# Создание бэкапа образа
sudo docker save -o /image_backup.tar nginx_image
```

3. **Восстановление бэкапа:**

```sh
# Восстановление контейнера из архива
sudo docker import /container_backup.tar

# Восстановление образа из архива
sudo docker load -i /image_backup.tar
```
