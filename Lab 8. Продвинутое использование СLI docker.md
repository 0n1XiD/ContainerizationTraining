# Практические задания

1. Запустите контейнер и посотрите информацию о состоянии docker на хосте. Выведите информацию о типе операционной системы на которой установлен docker в файл, этом используйте командку docker info.
2. Запустите произвольный контейнер. Выполните тестирование производительности в течении 3 минут. Оцените общую производительность во время выполения теста системы. Заморозьте контейнер. Разморозьте контейнер.
3. Посотрите логи веб сервера apache, когда контейнер находится под тестированием производительности.
4. Запустите еще один контейнер с mongodb. Посмотрите статистику всех конетйнеров, используя (--format), не включая столбец PIDS. Запишите все результаты в файл.
5. Изучите дополнительные параметры в тестировании производительности веб приложений.
6. Используя контейнер nginx, послитайте количество запросов к веб серверу за один день.

**1)**

С помощью команды `sudo docker info` выводим информацию в файл docker_info.txt:

```sh
sudo docker info > docker_info.txt
```

В файле получаем следующую информацию:

```
Client: Docker Engine - Community
 Version:    24.0.7
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.11.2
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.21.0
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 12
  Running: 9
  Paused: 0
  Stopped: 3
 Images: 22
 Server Version: 24.0.7
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 3dd1e886e55dd695541fdcd67420c2888645a495
 runc version: v1.1.10-0-g18a0cb0
 init version: de40ad0
 Security Options:
  apparmor
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.2.0-39-generic
 Operating System: Ubuntu 22.04.3 LTS
 OSType: linux
 Architecture: x86_64
 CPUs: 3
 Total Memory: 6.579GiB
 Name: onix-VirtualBox
 ID: 707bd9a7-a8bb-448e-903a-e162df5c2700
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```

**2)**

Проведем тест:

```sh
sudo docker run --rm jordi/ab -t 180 -c 10 http://localhost:90
```

Заморозку и разморозку контейнера можно осуществить, используя команды:

```sh
sudo docker pause perf-test-container
sudo docker unpause perf-test-container
```

**3)**

Во время тестирования из задания 2 проверим логи Apache:

```sh
sudo docker logs b9e8bf9964f8

[Thu Dec 21 20:06:11.203024 2023] [mpm_event:notice] [pid 1:tid 140536008918912] AH00489: Apache/2.4.58 (Unix) configured -- resuming normal operations
[Thu Dec 21 20:06:11.203179 2023] [core:notice] [pid 1:tid 140536008918912] AH00094: Command line: 'httpd -D FOREGROUND'
```

**4)**

Создадим контейнер Mongo:

```sh
sudo docker run -d --name mongodb-container mongo
```

После создания выведем всю необходимую статистику при помощи команды:

```sh
sudo docker stats --no-stream --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}\t{{.BlockIO}}" $(sudo docker ps -q) | grep -v "PIDS" > container_stats.txt
```

В созданном файле получим следующую информацию:

```
CONTAINER      CPU %     MEM USAGE / LIMIT     NET I/O           BLOCK I/O
b27465272ed3   0.41%     83.89MiB / 6.579GiB   3.16kB / 0B       11.1MB / 266kB
96e916c52992   0.00%     3.758MiB / 6.579GiB   7.53kB / 2.33kB   0B / 8.19kB
5d0a4affc9a1   0.00%     4.699MiB / 6.579GiB   6.41kB / 0B       1.13MB / 12.3kB
12462728e6b6   0.00%     6.242MiB / 6.579GiB   6.57kB / 0B       5.23MB / 16.4kB
326ad5443024   0.00%     27MiB / 6.579GiB      6.5kB / 0B        3.57MB / 41.6MB
8a730bb29278   0.00%     3.211MiB / 6.579GiB   6.77kB / 0B       0B / 12.3kB
b9e8bf9964f8   0.00%     6.125MiB / 50MiB      8.35kB / 0B       0B / 20.5kB
8e07906309bb   0.00%     3.055MiB / 6.579GiB   13.8kB / 0B       1.64MB / 4.1kB
30b23096bdbe   0.00%     32.78MiB / 6.579GiB   13.9kB / 0B       9.65MB / 692kB
7963ebad1bcd   0.01%     5.316MiB / 6.579GiB   14.1kB / 0B       1.62MB / 0B
```

**6)**

Создадим Веб-сервер:

```sh
sudo docker run -d -p 85:85 --name nginx-container nginx
```

После чего посчитаем количество запросов за день:

```sh
sudo docker exec nginx-container ab -n 100 -c 10 http://localhost/
```
