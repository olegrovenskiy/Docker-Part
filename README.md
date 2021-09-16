# Домашнее задание к занятию "5.4. Практические навыки работы с Docker"

## Задача 1

  Новый образ:


    FROM archlinux:latest
    
    RUN pacman -Syy --noconfirm ponysay

    ENTRYPOINT ["/usr/bin/ponysay"]
    CMD ["Hey, netology”]

результаты построения

  
ссылка на скриншот командной строки после запуска контейнера

https://clip2net.com/s/4djPoDD


ссылка на образ в репозитории


https://hub.docker.com/repository/docker/olegrovenskiy/ponysays-01.v1









