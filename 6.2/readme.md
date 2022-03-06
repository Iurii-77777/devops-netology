## **Задача 1.**
#### Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.
![Screenshot](1.jpg)
## **Задача 2.**
#### Итоговый список БД, описание таблиц (describe) SQL-запрос для выдачи списка пользователей с правами над таблицами test_db, список пользователей с правами над таблицами test_db.
```
CREATE DATABASE test_db
CREATE ROLE "test-admin-user" SUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;

CREATE TABLE orders 
(
id integer, 
name text, 
price integer, 
PRIMARY KEY (id) 
);

CREATE TABLE clients 
(
	id integer PRIMARY KEY,
	lastname text,
	country text,
	booking integer,
	FOREIGN KEY (booking) REFERENCES orders (Id)
);

CREATE ROLE "test-simple-user" NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;
GRANT SELECT ON TABLE public.clients TO "test-simple-user";
GRANT INSERT ON TABLE public.clients TO "test-simple-user";
GRANT UPDATE ON TABLE public.clients TO "test-simple-user";
GRANT DELETE ON TABLE public.clients TO "test-simple-user";
GRANT SELECT ON TABLE public.orders TO "test-simple-user";
GRANT INSERT ON TABLE public.orders TO "test-simple-user";
GRANT UPDATE ON TABLE public.orders TO "test-simple-user";
GRANT DELETE ON TABLE public.orders TO "test-simple-user";
```
![Screenshot](2.jpg)
## **Задача 3.**
#### Используя SQL синтаксис: вычислите количество записей для каждой таблицы. Приведите в ответе: запросы, результаты их выполнения.
```

```
## **Задача 4.**
#### Используя foreign keys свяжите записи из таблиц, согласно таблице. Приведите SQL-запросы для выполнения данных операций. Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.
```

```
## **Задача 5.**
#### Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN). Приведите получившийся результат и объясните что значат полученные значения.
```

```
## **Задача 6.**
#### Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов. Восстановите БД test_db в новом контейнере. Приведите список операций, который вы применяли для бэкапа данных и восстановления.
```

```
