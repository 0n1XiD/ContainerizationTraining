
#Практические задания:

* Установите Docker на свою операционную систему https://docs.docker.com/engine/install/.
* Создайте скрипт пошаговой установки Docker для своей системы, с целью дальнейшей автоматизации процесса. (Сохраните его для следующих занятий).
* Установите Docker внутрь минимального образа `alpine`.

**Задание 1 и 2:**

**1)**
Используя оффициальный сайт Docker установим Docker Engine на ОС Ubuntu 24.04

```sh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

После установки получим следующую информацию, выводимую в консоль:

``` sh
Сущ:1 http://ru.archive.ubuntu.com/ubuntu jammy InRelease
Пол:2 http://ru.archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Сущ:3 http://ru.archive.ubuntu.com/ubuntu jammy-backports InRelease
Пол:4 https://download.docker.com/linux/ubuntu jammy InRelease [48,8 kB]
Сущ:5 http://security.ubuntu.com/ubuntu jammy-security InRelease
Пол:6 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages [23,0 kB]
Получено 191 kB за 1с (276 kB/s)
Чтение списков пакетов… Готово
```

Установим Docker Packages

```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

После установки, проверим работу Docker, запустив image Hello World:

```sh
sudo docker run hello-world
```

После запуска, докер выдает нам приветственное сообщение:

```sh
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete 
Digest: sha256:ac69084025c660510933cca701f615283cdbb3aa0963188770b54c31c8962493
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

**2)**

После установки Docker на ОС, создаем файл Docker_install.sh и вписываем команды из 1-го задания, для автоматизации установки.


