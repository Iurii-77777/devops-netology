## **Задача 1.**
#### Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume. Подключитесь к БД PostgreSQL используя psql. Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.
```
iurii-devops@Host-SPB:~$ sudo docker pull postgres:13
[sudo] пароль для iurii-devops: 
13: Pulling from library/postgres
1efc276f4ff9: Pull complete 
66c520df917d: Pull complete 
5b124e6748c9: Pull complete 
1a06bb042d01: Pull complete 
e3849c675ec5: Pull complete 
d3b2eaf1435b: Pull complete 
074399829dc9: Pull complete 
6feb085525d8: Pull complete 
4153d17924d6: Pull complete 
bc311b90edd7: Pull complete 
9dab89a024b4: Pull complete 
e60b3f3ab3f2: Pull complete 
0091f9daf172: Pull complete 
Digest: sha256:03652c675ae177af98ddd50f9f4b4b2cf8ad38d0e116aa68fe670fbc2cf250fc
Status: Downloaded newer image for postgres:13
docker.io/library/postgres:13

iurii-devops@Host-SPB:~$ sudo docker volume create vol_postgres
vol_postgres

iurii-devops@Host-SPB:~$ sudo docker run --rm --name pg1 -e POSTGRES_PASSWORD=pass -ti -p 5432:5432 -v vol_postgres:/var/lib/postgresql/data postgres:13
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

iurii-devops@Host-SPB:~$ sudo docker exec -it pg1 bash
[sudo] пароль для iurii-devops: 
root@f7aecfc21276:/# psql -h localhost -p 5432 -U postgres -W
Password: 
psql (13.7 (Debian 13.7-1.pgdg110+1))
Type "help" for help.

postgres=#
```
#### Найдите и приведите управляющие команды для:
#### - вывода списка БД
#### - подключения к БД
#### - вывода списка таблиц
#### - вывода описания содержимого таблиц
#### - выхода из psql
```
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)

postgres=# \dtS
                    List of relations
   Schema   |          Name           | Type  |  Owner   
------------+-------------------------+-------+----------
 pg_catalog | pg_aggregate            | table | postgres
 pg_catalog | pg_am                   | table | postgres
 pg_catalog | pg_amop                 | table | postgres
 pg_catalog | pg_amproc               | table | postgres
 pg_catalog | pg_attrdef              | table | postgres
 pg_catalog | pg_attribute            | table | postgres
 pg_catalog | pg_auth_members         | table | postgres
 pg_catalog | pg_authid               | table | postgres
 pg_catalog | pg_cast                 | table | postgres
 pg_catalog | pg_class                | table | postgres
 pg_catalog | pg_collation            | table | postgres
 pg_catalog | pg_constraint           | table | postgres
 pg_catalog | pg_conversion           | table | postgres
 pg_catalog | pg_database             | table | postgres
 pg_catalog | pg_db_role_setting      | table | postgres
 pg_catalog | pg_default_acl          | table | postgres
 pg_catalog | pg_depend               | table | postgres
 pg_catalog | pg_description          | table | postgres
 pg_catalog | pg_enum                 | table | postgres
 pg_catalog | pg_event_trigger        | table | postgres
 pg_catalog | pg_extension            | table | postgres
 pg_catalog | pg_foreign_data_wrapper | table | postgres
 pg_catalog | pg_foreign_server       | table | postgres
 pg_catalog | pg_foreign_table        | table | postgres
 pg_catalog | pg_index                | table | postgres
 pg_catalog | pg_inherits             | table | postgres
 pg_catalog | pg_init_privs           | table | postgres
 pg_catalog | pg_language             | table | postgres
 pg_catalog | pg_largeobject          | table | postgres
 pg_catalog | pg_largeobject_metadata | table | postgres
 pg_catalog | pg_namespace            | table | postgres
 pg_catalog | pg_opclass              | table | postgres
 pg_catalog | pg_operator             | table | postgres
 pg_catalog | pg_opfamily             | table | postgres
 pg_catalog | pg_partitioned_table    | table | postgres
 pg_catalog | pg_policy               | table | postgres
 pg_catalog | pg_proc                 | table | postgres
 pg_catalog | pg_publication          | table | postgres
 pg_catalog | pg_publication_rel      | table | postgres
 pg_catalog | pg_range                | table | postgres
 pg_catalog | pg_replication_origin   | table | postgres
 pg_catalog | pg_rewrite              | table | postgres
 pg_catalog | pg_seclabel             | table | postgres
 pg_catalog | pg_sequence             | table | postgres
 pg_catalog | pg_shdepend             | table | postgres
 pg_catalog | pg_shdescription        | table | postgres
 pg_catalog | pg_shseclabel           | table | postgres
 pg_catalog | pg_statistic            | table | postgres
 pg_catalog | pg_statistic_ext        | table | postgres
 pg_catalog | pg_statistic_ext_data   | table | postgres
 pg_catalog | pg_subscription         | table | postgres
 pg_catalog | pg_subscription_rel     | table | postgres
 pg_catalog | pg_tablespace           | table | postgres
 pg_catalog | pg_transform            | table | postgres
 pg_catalog | pg_trigger              | table | postgres
 pg_catalog | pg_ts_config            | table | postgres
 pg_catalog | pg_ts_config_map        | table | postgres
 pg_catalog | pg_ts_dict              | table | postgres
 pg_catalog | pg_ts_parser            | table | postgres
 pg_catalog | pg_ts_template          | table | postgres
 pg_catalog | pg_type                 | table | postgres
 pg_catalog | pg_user_mapping         | table | postgres
(62 rows)

postgres=# \dS+ pg_index
                                      Table "pg_catalog.pg_index"
     Column     |     Type     | Collation | Nullable | Default | Storage  | Stats target | Description 
----------------+--------------+-----------+----------+---------+----------+--------------+-------------
 indexrelid     | oid          |           | not null |         | plain    |              | 
 indrelid       | oid          |           | not null |         | plain    |              | 
 indnatts       | smallint     |           | not null |         | plain    |              | 
 indnkeyatts    | smallint     |           | not null |         | plain    |              | 
 indisunique    | boolean      |           | not null |         | plain    |              | 
 indisprimary   | boolean      |           | not null |         | plain    |              | 
 indisexclusion | boolean      |           | not null |         | plain    |              | 
 indimmediate   | boolean      |           | not null |         | plain    |              | 
 indisclustered | boolean      |           | not null |         | plain    |              | 
 indisvalid     | boolean      |           | not null |         | plain    |              | 
 indcheckxmin   | boolean      |           | not null |         | plain    |              | 
 indisready     | boolean      |           | not null |         | plain    |              | 
 indislive      | boolean      |           | not null |         | plain    |              | 
 indisreplident | boolean      |           | not null |         | plain    |              | 
 indkey         | int2vector   |           | not null |         | plain    |              | 
 indcollation   | oidvector    |           | not null |         | plain    |              | 
 indclass       | oidvector    |           | not null |         | plain    |              | 
 indoption      | int2vector   |           | not null |         | plain    |              | 
 indexprs       | pg_node_tree | C         |          |         | extended |              | 
 indpred        | pg_node_tree | C         |          |         | extended |              | 
Indexes:
    "pg_index_indexrelid_index" UNIQUE, btree (indexrelid)
    "pg_index_indrelid_index" btree (indrelid)
Access method: heap

postgres=# \q
root@f7aecfc21276:/#
```
## **Задача 2.**
#### Используя psql создайте БД test_database. Изучите бэкап БД. Восстановите бэкап БД в test_database. Перейдите в управляющую консоль psql внутри контейнера.
```

```
#### Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице. Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах. Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.
```
```
## **Задача 3.**
#### Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).
```
```
#### Предложите SQL-транзакцию для проведения данной операции. Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?
```
```
## **Задача 4.**
#### Используя утилиту pg_dump создайте бекап БД test_database.
```
```
#### Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?
```
```
