#Практические задания

1. Повторите практические примеры для образа веб-сервера nginx, измените стартовую html страницу произвольным образом
2. Повторите практические примеры для образа веб-сервера apache, измените стартовую html страницу произвольным образом


**1)**
Запустим образ nginx и примапим к хост машине и назовем proxy, добавив параметр автоматического рестарта контейнера при остановке:

```sh
sudo docker container run -d -p 80:80 --name proxy --restart always nginx
```

После чего получим справку о метаданных, изначально отформатировав её с помощью команд:

```sh
docker container inspect proxy --format "IP: {{ .NetworkSettings.IPAddress }} | Gateway: {{.NetworkSettings.Networks.bridge.Gateway}}"
docker container inspect
```

Получим следующую информацию:

```sh
[
    {
        "Id": "aa4c5d664d2e9454559be38a93433ba1ab5def6b9d77f431c7c380c550aa5c2a",
        "Created": "2023-12-21T19:17:56.562220344Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 8410,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2023-12-21T19:17:57.480264446Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:d453dd892d9357f3559b967478ae9cbc417b52de66b53142f6c16c8a275486b9",
        "ResolvConfPath": "/var/lib/docker/containers/aa4c5d664d2e9454559be38a93433ba1ab5def6b9d77f431c7c380c550aa5c2a/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/aa4c5d664d2e9454559be38a93433ba1ab5def6b9d77f431c7c380c550aa5c2a/hostname",
        "HostsPath": "/var/lib/docker/containers/aa4c5d664d2e9454559be38a93433ba1ab5def6b9d77f431c7c380c550aa5c2a/hosts",
        "LogPath": "/var/lib/docker/containers/aa4c5d664d2e9454559be38a93433ba1ab5def6b9d77f431c7c380c550aa5c2a/aa4c5d664d2e9454559be38a93433ba1ab5def6b9d77f431c7c380c550aa5c2a-json.log",
        "Name": "/proxy",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "80"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "always",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                24,
                80
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/90f1b7042407aa9baf11bca6c5a68c07e257acfe3a5d0677795d26ca3d8c9f35-init/diff:/var/lib/docker/overlay2/16f08e61b077174cbe836a6d91c424b37246e5e1279c55ea2cf647e7e2e93599/diff:/var/lib/docker/overlay2/34d9905ea5d238cccd7e79b1ff5d032d3d5eef0d372ac52bd38676655c9bcfdc/diff:/var/lib/docker/overlay2/d2d3e3c0d2da8e7c43cee0d168d351aebc562a5a9e248cda8d5eabeecd4f8a4a/diff:/var/lib/docker/overlay2/8ae3fcd6a8415b9be1221c2b39938748926f4540d6c3fe4a3b9cfe55b19afb35/diff:/var/lib/docker/overlay2/529b4f011d48f9079fbdfdc494e3e8ea401e09aa7050baec813a900db4d7432d/diff:/var/lib/docker/overlay2/6773f2dff215d4ddad240d4c943529c8e73b2b28d839f5cb170a723dc41b5549/diff:/var/lib/docker/overlay2/f15b543f4ade90e131056d6b763318760377ca40635691774c1e0fbaa57fbd91/diff",
                "MergedDir": "/var/lib/docker/overlay2/90f1b7042407aa9baf11bca6c5a68c07e257acfe3a5d0677795d26ca3d8c9f35/merged",
                "UpperDir": "/var/lib/docker/overlay2/90f1b7042407aa9baf11bca6c5a68c07e257acfe3a5d0677795d26ca3d8c9f35/diff",
                "WorkDir": "/var/lib/docker/overlay2/90f1b7042407aa9baf11bca6c5a68c07e257acfe3a5d0677795d26ca3d8c9f35/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "aa4c5d664d2e",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.25.3",
                "NJS_VERSION=0.8.2",
                "PKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "136a1134c2d721060baaa47c7c453c7686296755bd2d17999b5dad2ee358635f",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "80"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "80"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/136a1134c2d7",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "9cdbe10ae392f7bdeef6db3224253db724c6057182dc852c7abec40e70bca7a5",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "ed22c642af2e004a0d938d94e7eb68e64de8b6056fbe06e5144933086850fa66",
                    "EndpointID": "9cdbe10ae392f7bdeef6db3224253db724c6057182dc852c7abec40e70bca7a5",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

```

Переименуем контейнер:

```sh
docker container rename proxy proxy_new
```

Для изменения стартовой страницы HTML зайдем внутрь контейнера proxy_new, с помощью команды:

```sh
sudo docker container exec -it proxy_new bash
```

После чего в интерфейсе CLI перейдем в директорию с HTML файлом. Для этого используем команду:

```sh
cd /usr/share/nginx/html/   
```

Откроем файл `index.html`, получив следующий код страницы:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Изменим его на следующий код:

``` 
<!DOCTYPE html>
<html>
<body>
<h1>Делаю 2 лабу/h1>
</body>
</html>
```

После открытия страницы сервера, страница будет изменена на нашу.

**2)**

Для образа Apache схема создания аналогичная, за исключением смены HTML, т.к. index.html, находится в /var/www/html/index.html

```bash
cd /var/www/html/index.html
```
