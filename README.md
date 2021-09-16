# Домашнее задание к занятию "5.4. Практические навыки работы с Docker"

## Задача 1

  Новый образ:


    FROM archlinux:latest
    
    RUN pacman -Syy --noconfirm ponysay

    ENTRYPOINT ["/usr/bin/ponysay"]
    CMD ["Hey, netology”]

результаты построения

  c:\Oleg>docker build -t test -f dockerfile.txt .
  [+] Building 11.8s (6/6) FINISHED
   => [internal] load build definition from dockerfile.txt                                                           0.1s
   => => transferring dockerfile: 165B                                                                               0.0s
   => [internal] load .dockerignore                                                                                  0.1s
   => => transferring context: 2B                                                                                    0.0s
   => [internal] load metadata for docker.io/library/archlinux:latest                                                0.0s
   => CACHED [1/2] FROM docker.io/library/archlinux:latest                                                           0.0s
   => [2/2] RUN pacman -Syy --noconfirm ponysay                                                                     10.0s
   => exporting to image                                                                                             1.5s
  => => exporting layers                                                                                            1.4s
   => => writing image sha256:9073c1ec0d2c5db0c5bee4c07274a3bbe0cd4065922a1f06a4452951f5c9c4d2                       0.0s
   => => naming to docker.io/library/test                                                                            0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them

  c:\Oleg>docker images
  REPOSITORY               TAG       IMAGE ID       CREATED          SIZE
  test                     latest    9073c1ec0d2c   2 minutes ago    469MB
  <none>                   <none>    281a4bc6dd9f   26 minutes ago   461MB

ссылка на скриншот командной строки после запуска контейнера

https://clip2net.com/s/4djPoDD


  c:\Oleg>docker ps -a
  CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                     PORTS                               NAMES
  0bb3ac6f8d19   test                     "/usr/bin/ponysay /b…"   8 minutes ago   Exited (0) 8 minutes ago                                       pony1
  ba7167c6c0d3   archlinux                "/usr/bin/bash"          44 hours ago    Up 44 hours                                                    arch
  64327697e578   docker/getting-started   "/docker-entrypoint.…"   2 days ago      Up 2 days                  0.0.0.0:80->80/tcp, :::80->80/tcp   musing_boyd

Сборка нового образа и пуш его в репозиторий

  c:\Oleg>docker commit -m "archlinux-ponysaya" -a "Oleg Rovenskiy" 0bb3ac6f8d19 ponysays-al
  sha256:4be9438282ac16c4f29da864d99aca2a450d339646d698443a26d86745e00442

  c:\Oleg>
  c:\Oleg>
  c:\Oleg>docker images
  REPOSITORY               TAG       IMAGE ID       CREATED          SIZE
  ponysays-al              latest    4be9438282ac   13 seconds ago   469MB
  test                     latest    9073c1ec0d2c   17 minutes ago   469MB
  <none>                   <none>    281a4bc6dd9f   41 minutes ago   461MB
  archlinux                latest    1d6f90387c13   13 days ago      381MB
  docker/getting-started   latest    083d7564d904   3 months ago     28MB

  c:\Oleg>


  c:\Oleg>docker tag ponysays-al olegrovenskiy/ponysays-01.v1

  c:\Oleg>docker images
  REPOSITORY                     TAG       IMAGE ID       CREATED          SIZE
  ponysays-al                    latest    4be9438282ac   5 minutes ago    469MB
  olegrovenskiy/ponysays-01.v1   latest    4be9438282ac   5 minutes ago    469MB
  test                           latest    9073c1ec0d2c   22 minutes ago   469MB
  <none>                         <none>    281a4bc6dd9f   46 minutes ago   461MB
  archlinux                      latest    1d6f90387c13   13 days ago      381MB
  docker/getting-started         latest    083d7564d904   3 months ago     28MB

  c:\Oleg>docker push olegrovenskiy/ponysays-01.v1
  Using default tag: latest
  The push refers to repository [docker.io/olegrovenskiy/ponysays-01.v1]
  f68ceb55d93f: Pushed
  1f3b1c7bc202: Mounted from library/archlinux
  959ee6191ad6: Mounted from library/archlinux
  latest: digest: sha256:029eb269fdca80d537c800e933f3aea6a42d0ea9745ada85515eaa2b2f4ffdcb size: 950


ссылка на образ в репозитории


https://hub.docker.com/repository/docker/olegrovenskiy/ponysays-01.v1









