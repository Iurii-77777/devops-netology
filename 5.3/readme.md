## **Задача 1. Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки**
```
https://hub.docker.com/repository/docker/iurii88888/netology2022

```
## **Задача 2. Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина?**
```
1. Высоконагруженное монолитное java веб-приложение - физический сервер.
2. Nodejs веб-приложение - докер.
3. Мобильное приложение c версиями для Android и iOS - виртуальный сервер.
4. Шина данных на базе Apache Kafka - возможен докер.
5. Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana - кластерная виртуалка для elasticsearch, остальное на докере.
6. Мониторинг-стек на базе Prometheus и Grafana - докер.
7. MongoDB, как основное хранилище данных для java-приложения - виртуалка или физ. сервер.
8. Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry - докер.
```
## **Задача 3. Листинг и содержание файлов в /data контейнерах**
```

```
## **Задача 4. Соберите Docker образ с Ansible, загрузите на Docker Hub и пришлите ссылку вместе с остальными ответами к задачам.**
```
root@Host-SPB:~# docker run -it -v /root/data:/root/data --name debian.1 -d debian      #запускаем контейнер c debian
8edea988767a4a900addc8157cb7a0e1fc1d62b5954752b9f0d001c5fc35b9b1

root@Host-SPB:~# docker ps                                                                      #проверяем контейнер
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
8edea988767a   debian    "bash"    5 seconds ago   Up 5 seconds             debian.1

root@Host-SPB:~# docker run -it -v /root/data:/root/data --name centos.1 -d centos              #запускаем контейнер с centos
0ff42acd9fb60fcf9bb737f426728b28f0daaaecfae7e42ad927e5d04c41cda2

root@Host-SPB:~# docker ps                                                                      #проверяем контейнеры debian и centos
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
0ff42acd9fb6   centos    "/bin/bash"   2 seconds ago    Up 1 second               centos.1
8edea988767a   debian    "bash"        38 seconds ago   Up 38 seconds             debian.1

root@Host-SPB:~# docker exec -it debian.1 /bin/bash                                             #подключаемся к debian контейнеру и создаём тестовый файл
  root@8edea988767a:/# ls
  bin  boot  dev	etc  home  lib	lib64  media  mnt  opt	proc  root  run  sbin  srv  sys  tmp  usr  var
  root@8edea988767a:/# cd root/
  root@8edea988767a:~# ls
  data
  root@8edea988767a:~# cd data/
  root@8edea988767a:~/data# touch my-test-file
  root@8edea988767a:~/data# echo debian_for_netology_homework > my-test-file 
  root@8edea988767a:~/data# exit
  exit

root@Host-SPB:~# ls                   #видим файл в хостовой машине
data  docker-test  snap
root@Host-SPB:~# cd data/
root@Host-SPB:~/data# ls
my-test-file

root@Host-SPB:~# docker exec -it centos.1 /bin/bash                 #подключаемся к контейнеру centos
  [root@0ff42acd9fb6 /]# ls
  bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
  [root@0ff42acd9fb6 /]# cd root
  [root@0ff42acd9fb6 ~]# cd data/
  [root@0ff42acd9fb6 data]# ls
  my-test-file
  [root@0ff42acd9fb6 data]# cat my-test-file                        #видим файл в данном контейнере
  debian_for_netology_homework

```
