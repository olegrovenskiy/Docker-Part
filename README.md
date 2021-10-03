# Домашнее задание к занятию "5.4. Практические навыки работы с Docker"

## Задача 1

  Новый образ:


    FROM archlinux:latest
    
    RUN pacman -Syy --noconfirm ponysay

    ENTRYPOINT ["/usr/bin/ponysay"]
    CMD ["Hey, netology”]

  
ссылка на скриншот командной строки после запуска контейнера

https://clip2net.com/s/4djPoDD


ссылка на образ в репозитории


https://hub.docker.com/repository/docker/olegrovenskiy/ponysays-01.v1


## Задача 2

### Пункт 2.1  --  Из образа на основе -- образ - amazoncorretto

Докер файл

    FROM amazoncorretto
    
    RUN \
        yum install wget -y && \
        yum update && \
        wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo && \
        rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key && \
        yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y && \
        yum install jenkins -y && \
        yum -y install initscripts
    
    
    EXPOSE 8080
    EXPOSE 50000



Скриншот докер лога

https://c2n.me/4dxjUhS

![докер лог](Docker-Part/2.1-log.png)

    c:\Oleg\version1>docker logs jver1
    bash-4.2#
    bash-4.2#
    bash-4.2#
    bash-4.2#
    bash-4.2# service jenkins start
    Failed to get D-Bus connection: Operation not permitted
    Starting Jenkins                                           [  OK  ]
    
    c:\Oleg\version1>
    c:\Oleg\version1>
    c:\Oleg\version1>


Команды построения образа и запуска контейнера

    docker build -t jen-ver2 -f dockerfile .
    docker run -it --name jver1 -p 7777:8080 version1

Запуск сервере jenkins из докер контейнера

    bash-4.2#
    bash-4.2# service jenkins start
    Failed to get D-Bus connection: Operation not permitted
    Starting Jenkins                                           [  OK  ]
    bash-4.2#


Скриншот в браузере по 7777 порту

https://c2n.me/4dxjZhD


Ссылка на образ в хранилище docker-hub

https://hub.docker.com/repository/docker/olegrovenskiy/jenkins-am

-----------------------------------------------------


### Пункт 2.2  --  Из образа на основе -- образ - ubuntu:latest


Докер файл


        FROM ubuntu:latest
        
        ENV TZ=Europe/Moscow
        
        RUN \
            apt-get update && \
            apt-get install wget -y && \
            apt-get install gnupg -y && \
            wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | apt-key add - && \
            sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' && \
            apt-get update && \
            apt-get install jenkins -y && \
            apt-get install openjdk-8-jdk -y
    
       EXPOSE 8080
       EXPOSE 50000


Лог запуска контейнера


https://c2n.me/4dwZUG5

Команда запуска сервера jenkins из контейнера

        root@23860013ccad:/# service jenkins start
        Correct java version found
         * Starting Jenkins Automation Server jenkins                                                                    [ OK ]
        root@23860013ccad:/#


Скриншот веб интерфейса при запуске

https://c2n.me/4dwZXyC

Пароль и вводд его

        root@23860013ccad:/# cat /var/lib/jenkins/secrets/initialAdminPassword
        8fbfbaa19f884e67aeab59111541c133
        root@23860013ccad:/#

https://c2n.me/4dx0p45

И после запуска

https://c2n.me/4dx0rHT


    c:\Oleg\Jenkins-Ver2>docker logs ver2
    root@23860013ccad:/#
    root@23860013ccad:/#
    root@23860013ccad:/#
    root@23860013ccad:/#
    root@23860013ccad:/# service jenkins start
    Correct java version found
     * Starting Jenkins Automation Server jenkins                                                                    [ OK ]
    root@23860013ccad:/#
    root@23860013ccad:/#
    root@23860013ccad:/# cat /var/lib/jenkins/secrets/initialAdminPassword
    8fbfbaa19f884e67aeab59111541c133
    
    c:\Oleg\Jenkins-Ver2>
    
    
    docker commit -m "jenkins-2" -a "Oleg Rovenskiy" ab48a6d98930 jenkins-version2

ссылка на образ в докер хабе

https://hub.docker.com/repository/docker/olegrovenskiy/jenkins-version2

Команды построения образа и запуска контейнера

    docker build -t jen-ver2 -f dockerfile .
    
    docker run --name ver2 -it -p 8222:8080 -p 50000:50000 jen-ver2

Состояние контейнера

    c:\Oleg\Jenkins-Ver2>docker ps -a
    CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                     PORTS                                                                                         NAMES
    4d822c28ca2a   jen-ver2                 "bash"                   20 minutes ago   Up 20 minutes              0.0.0.0:50000->50000/tcp, :::50000->50000/tcp, 0.0.0.0:8222-     >8080/tcp, :::8222->8080/tcp   ver2

----------------------------

## Задача 3

Докерфайл

    FROM node
    
    ARG APP_DIR=app
    RUN mkdir -p ${APP_DIR}
    WORKDIR ${APP_DIR}
    
    
    COPY package*.json ./
    
    RUN npm install
    
    COPY . .
    
    EXPOSE 3000
    
    CMD [ "npm", "start" ]

Файлы приложения

    package.json этого проекта:
    
    {
      "name": "node-app",
      "version": "1.0.0",
      "description": "The best way to manage your Node app using Docker",
      "main": "index.js",
      "scripts": {
        "start": "node index.js"
      },
      "author": "oleg rov <roleg_n@mail.ru>",
      "license": "ISC",
      "dependencies": {
      "express": "^4.16.4"
      }
    }


index.js, в котором находится код проекта:

    const express = require('express');
    const app = express();
    app.get('/', (req, res) => {
    res.send('The best way to manage your Node app using Docker\n');
    });
    app.listen(3000);
    console.log('Running on http://localhost:3000');

Команда построения образа

    docker build --build-arg APP_DIR=c:/oleg/network -t node1 .

Образ построен

    c:\Oleg\Network>docker images
    REPOSITORY                       TAG       IMAGE ID       CREATED          SIZE
    node1                            latest    c44aaf4972ed   39 seconds ago   923MB

Запуск контейнера

    c:\Oleg\Network>docker run -it --name tt123 -p 3000:3000 node1
    
    > node-app@1.0.0 start
    > node index.js
    
    Running on http://localhost:3000

Контейнер запустился

    c:\Oleg\Network>docker ps -a
    CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                      PORTS                                                                                          NAMES
    7be273b59866   node1                    "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes                0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                     tt123

проверка

    c:\Oleg\Network>curl localhost:3000
    The best way to manage your Node app using Docker
    
    c:\Oleg\Network>
    
    c:\Oleg\Network>curl -i localhost:3000
    HTTP/1.1 200 OK
    X-Powered-By: Express
    Content-Type: text/html; charset=utf-8
    Content-Length: 50
    ETag: W/"32-maar4Rfmmpiaz6uTJvDbUYgwGzk"
    Date: Sun, 03 Oct 2021 07:42:03 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5
    
    The best way to manage your Node app using Docker
    
    c:\Oleg\Network>

Использовались материалы

https://habr.com/ru/company/ruvds/blog/440656/


Запуск второго контейнера

    c:\Oleg\Network>docker run -it --name tt456 ubuntu
    
    c:\Oleg\Network>docker ps -a
    CONTAINER ID   IMAGE                    COMMAND                  CREATED              STATUS                      PORTS                                                                                         NAMES
    7093fe05d8f1   ubuntu                   "bash"                   About a minute ago   Up About a minute                                                                                                          tt456
    7be273b59866   node1                    "docker-entrypoint.s…"   10 minutes ago       Up 10 minutes               0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                  tt123


создание сети

    docker network create netology
    
    a66b55e3816c   netology          bridge    local

Подключение контейнеров к сети

    docker network connect netology tt123
    
    docker network connect netology tt456

Вывод команды network inspect

    c:\Oleg\Network>docker network inspect netology
    [
       {
           "Name": "netology",
           "Id": "a66b55e3816cccfd61da79ebb5983b1e2ff198026b7c7b723327b3be1a85cab2",
            "Created": "2021-10-03T07:51:39.599575901Z",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
               "Driver": "default",
                "Options": {},
                "Config": [
                   {
                        "Subnet": "172.20.0.0/16",
                       "Gateway": "172.20.0.1"
                    }
                ]
           },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
               "Network": ""
           },
           "ConfigOnly": false,
            "Containers": {
                "7093fe05d8f1878ba78a469927d187b51363b3d1cadfe087ae3267c831cbb35a": {
                    "Name": "tt456",
                    "EndpointID": "05994dee6da1220e7597c480a330baebf5dbe9b7375c963a63ba0d37a3ec04da",
                    "MacAddress": "02:42:ac:14:00:03",
                    "IPv4Address": "172.20.0.3/16",
                    "IPv6Address": ""
                },
               "7be273b598668d780d1e01857a0f4f2c4588a34431d8c89e94d5bf70f499cddb": {
                   "Name": "tt123",
                    "EndpointID": "1192edf2f50c97262f297dc7a3684a83d54bf4f16070169dd095d01668aaa8fe",
                    "MacAddress": "02:42:ac:14:00:02",
                    "IPv4Address": "172.20.0.2/16",
                    "IPv6Address": ""
                }
           },
           "Options": {},
            "Labels": {}
        }
    ]
    
    c:\Oleg\Network>

можно узнать ip address соответствующего контейнера

Далее можно сделать curl с контейнера ubuntu на контейнер node

    root@7093fe05d8f1:/# curl 172.20.0.2:3000
    The best way to manage your Node app using Docker
    root@7093fe05d8f1:/# curl -i 172.20.0.2:3000
    HTTP/1.1 200 OK
    X-Powered-By: Express
    Content-Type: text/html; charset=utf-8
    Content-Length: 50
    ETag: W/"32-maar4Rfmmpiaz6uTJvDbUYgwGzk"
    Date: Sun, 03 Oct 2021 08:01:01 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5
    
    The best way to manage your Node app using Docker
    root@7093fe05d8f1:/#
    
то есть сеть между двумя контейнерами работает

После установки iputils можно заустить пинг между контейнерами

    root@7093fe05d8f1:/# ping 172.20.0.2
    PING 172.20.0.2 (172.20.0.2) 56(84) bytes of data.
    64 bytes from 172.20.0.2: icmp_seq=1 ttl=64 time=2.74 ms
    64 bytes from 172.20.0.2: icmp_seq=2 ttl=64 time=0.121 ms
    ^C
    --- 172.20.0.2 ping statistics ---
    2 packets transmitted, 2 received, 0% packet loss, time 1002ms
    rtt min/avg/max/mdev = 0.121/1.430/2.739/1.309 ms
    root@7093fe05d8f1:/#

Скриншот вывода вызова команды списка docker сетей (docker network cli)

https://c2n.me/4dx5zWs

Скриншот вызова утилиты curl с успешным ответом

https://c2n.me/4dx5Djc


















