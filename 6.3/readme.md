## **Задача 1.**
#### Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume. Полный список команд.
```
iurii-devops@Host-SPB:~$ sudo docker pull mysql:8.0
[sudo] пароль для iurii-devops: 
8.0: Pulling from library/mysql
15115158dd02: Pull complete 
d733f6778b18: Pull complete 
1cc7a6c74a04: Pull complete 
c4364028a805: Pull complete 
82887163f0f6: Pull complete 
28abcb7f57e0: Pull complete 
46d27a431703: Pull complete 
8e745fe86aaf: Pull complete 
ab75add93486: Pull complete 
09e3960383f3: Pull complete 
59f780965951: Pull complete 
8ead2303095c: Pull complete 
Digest: sha256:b17a66b49277a68066559416cf44a185cfee538d0e16b5624781019bc716c122
Status: Downloaded newer image for mysql:8.0
docker.io/library/mysql:8.0

iurii-devops@Host-SPB:~$ sudo docker volume create vol_mysql
vol_mysql

iurii-devops@Host-SPB:~$ sudo docker run --rm --name mysql-docker -e MYSQL_ROOT_PASSWORD=netology -ti -p 5000:5000 -v vol_mysql:/etc/mysql/ mysql:8.0

```
## **Задача 2.**
#### Создайте пользователя test в БД c паролем test-pass используя установленные параметры. 
```
```
#### Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю test
## **Задача 3.**
#### Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.
#### Исследуйте, какой engine используется в таблице БД test_db.
#### Измените engine и приведите время выполнения и запрос на изменения из профайлера.
```
```
## **Задача 4.**
#### Изучите файл my.cnf в директории /etc/mysql.
#### Измените его согласно ТЗ (движок InnoDB).
```
```
![Screenshot](1.jpg)
