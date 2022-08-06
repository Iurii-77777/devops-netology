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

2022-08-03T21:07:00.984523Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
2022-08-03T21:07:00.984540Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.30'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
```
#### Переходим в управляющую консоль  :
```
iurii-devops@Host-SPB:~$ sudo docker exec -it mysql-docker mysql -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.30 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.
```
#### Получаем список управляющих команд:
```
mysql> \h

For information about MySQL products and services, visit:
   http://www.mysql.com/
For developer information, including the MySQL Reference Manual, visit:
   http://dev.mysql.com/
To buy MySQL Enterprise support, training, or other products, visit:
   https://shop.mysql.com/

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.
resetconnection(\x) Clean session context.
query_attributes Sets string parameters (name1 value1 name2 value2 ...) for the next query to pick up.
ssl_session_data_print Serializes the current SSL session data to stdout or file

For server side help, type 'help contents'
```
#### Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД.
```
mysql> \s
--------------
mysql  Ver 8.0.30 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:		8
Current database:	
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		8.0.30 MySQL Community Server - GPL
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	latin1
Conn.  characterset:	latin1
UNIX socket:		/var/run/mysqld/mysqld.sock
Binary data as:		Hexadecimal
Uptime:			31 min 2 sec

Threads: 2  Questions: 8  Slow queries: 0  Opens: 119  Flush tables: 3  Open tables: 38  Queries per second avg: 0.004
```
#### Подключитесь к восстановленной БД и получите список таблиц из этой БД.
```
sudo docker exec -i mysql-docker /usr/bin/mysql -u root --password=netology test_db < test_dump.sql

iurii-devops@Host-SPB:~$ sudo docker exec -it mysql-docker mysql --password=netology
mysql> use test_db
Database changed
mysql> show tables;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)
```
#### Приведите в ответе количество записей с price > 300.
```
mysql> select count(*) from orders where price >300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.01 sec)
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
