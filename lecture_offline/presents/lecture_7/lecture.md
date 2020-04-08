# Основы баз данных
!!!
## Основы баз данных
!!!
## История развития баз данных
![История](./images/history.jpg)
!!!

## Классификация БД по организации модели
- реляционные
- NoSQL
- Графовые

!!!

## NoSQL
Под лэйблом [NoSQL](http://nosql-database.org/) сейчас скрывается множество разнородных систем

!!!

## NoSQL
- Не используется SQL
- Неструктурированные (schemaless)
- Представление данных в виде агрегатов (aggregates)
- NoSQL базы в основном оупенсорсные

!!!

## NoSQL
![NoSQL](./images/nosql-databases.gif)

!!!

## Графовые базы данных
- Отсутствие единой схемы всех элементов
- Можно хранить и реляционные, и документарные данные
- Модель может меняться без изменения архитектуры

!!!

## Графовые базы данных
![graph](./images/graphDB.png).

!!!

## Особенности РБД
- Отображает информацию в наиболее простой для пользователя форме
- Основана на развитом математическом аппарате
- Манипулирование данными на уровне выходной БД и возможность изменения.

!!!

## Реляционные
![SQL](./images/sql.png)

!!!

## Отличие реляционных БД от всех
- Требуют наличия однозначно определённой структуры хранения данных
- Вне зависимости от лицензии, РСУБД реализуют SQL-стандарты
- Масштабируемость
- Надежны
- Хорошая поддержка
- Предполагают работу с сложными ситуациями

!!!

## Вопросы

!!!

## Примитивы баз данных
![структура](./images/structure.jpg)

!!!

## Знакомство с SQL

!!!

## История развития
| Год  | Название | Изменения |
| ---- | -------- | --------- |
| 1986  | SQL-86  | Первый вариант стандарта   |
| 1989  | SQL-89  |  Немного доработанный вариант предыдущего стандарта.  |
| 1992  | SQL-92  |  Значительные изменения (ISO 9075)  |
| 1999  | SQL:1999  |  Добавлена поддержка регулярных выражений, рекурсивных запросов, поддержка триггеров, базовые процедурные расширения, нескалярные типы данных и некоторые объектно-ориентированные возможности.  |
| 2003  | SQL:2003  |  Введены расширения для работы с XML-данными, оконные функции (применяемые для работы с OLAP-базами данных), генераторы последовательностей и основанные на них типы данных.  |
| 2006  | SQL:2006  |  Функциональность работы с XML-данными значительно расширена. Появилась возможность совместно использовать в запросах SQL и XQuery.  |
| 2008  | SQL:2008  |  Улучшены возможности оконных функций, устранены некоторые неоднозначности стандарта SQL:2003[5]|

!!!

## Операторы SQL
- операторы определения данных (Data Definition Language, DDL)
- операторы манипуляции данными (Data Manipulation Language, DML)
- операторы определения доступа к данным (Data Control Language, DCL)
- операторы управления транзакциями (Transaction Control Language, TCL)

!!!

## DDL
- CREATE
- ALTER
- DROP

!!!

## DML
- SELECT
- INSERT
- UPDATE
- DELETE

!!!

## DCL
- GRANT
- REVOKE
- DENY

!!!

## TCL
- COMMIT
- ROLLBACK
- SAVEPOINT

!!!

## Типы данных SQL
— строковые (CHAR, VARCHAR, BLOB)
— с плавающей точкой (FLOAT, DOUBLE, DECIMAL)
— целые числа, дата и время (INT, DATE, DATETIME)

!!!

## Создание таблиц
```
CREATE TABLE table_name (
 column_name TYPE column_constraint,
 table_constraint table_constraint
) INHERITS existing_table_name;
```

!!!

## Пример
```
CREATE TABLE account(
 user_id serial PRIMARY KEY,
 username VARCHAR (50) UNIQUE NOT NULL,
 password VARCHAR (50) NOT NULL,
 email VARCHAR (355) UNIQUE NOT NULL,
 created_on TIMESTAMP NOT NULL,
 last_login TIMESTAMP
);
```
!!!

## Пример
![create](./images/CREATE.jpg)

!!!

## Заполение данными
```
INSERT INTO table(column1, column2, …)
VALUES
 (value1, value2, …);
```

!!!

## Пример
```
CREATE TABLE link (
 ID serial PRIMARY KEY,
 url VARCHAR (255) NOT NULL,
 name VARCHAR (255) NOT NULL,
 description VARCHAR (255),
 rel VARCHAR (50)
);
```
!!!

## Вставка одной строки
```
INSERT INTO link (url, name)
VALUES
 ('http://www.postgresqltutorial.com','PostgreSQL Tutorial');
```
!!!

## Вставка нескольких строк
```
INSERT INTO link (url, name)
VALUES
 ('http://www.google.com','Google'),
 ('http://www.yahoo.com','Yahoo'),
 ('http://www.bing.com','Bing');
```
!!!

## Получение данных
```
SELECT
 column_1,
 column_2,
 ...
FROM
 table_name;
```
!!!

## Пример
![customer](./images/customer-table.png)

!!!

## Пример
```
SELECT
 *
FROM
 customer;
```

!!!

## Пример
```
SELECT
 first_name,
 last_name,
 email
FROM
 customer;
 ```
!!!

## Условия в запросах
```
SELECT column_1, column_2 … column_n
FROM table_name
WHERE conditions;
```
!!!

## Условия в запросах
![ex](./images/table-if.png)

!!!

## Пример 1
```
SELECT last_name, first_name
FROM customer
WHERE first_name = 'Jamie';
```
!!!

## Пример 2
```
SELECT last_name, first_name
FROM customer
WHERE first_name = 'Jamie' AND
 last_name = 'Rice';
 ```
 !!!

## Пример 3
```
SELECT customer_id,
 amount,
 payment_date
FROM payment
WHERE amount <= 1 OR amount >= 8;

```
!!!

## Выполнение подзапросов
```
value IN (value1,value2,...)
```

!!!

## Выполнение подзапросов
```
value IN (SELECT value FROM tbl_name);
```

!!!

## Выполнение подзапросов
```
SELECT
 first_name,
 last_name
FROM
 customer
WHERE
 customer_id IN (
 SELECT
 customer_id
 FROM
 rental
 WHERE
 CAST (return_date AS DATE) = '2005-05-27'
 );
```

!!!

## Обновление данных
```
UPDATE table
SET column1 = value1,
    column2 = value2 ,...
WHERE
 condition;
```

!!!

## Пример
```
UPDATE link
SET last_update = DEFAULT
WHERE
 last_update IS NULL;
```
!!!
## Пример (обновление всех строк)
```
UPDATE link
SET rel = 'nofollow';
```
!!!
## Пример (обновление с возвращением)
```
UPDATE link
SET description = 'Learn PostgreSQL fast and easy',
 rel = 'follow'
WHERE
 ID = 1
RETURNING id,
 description,
 rel;
```
!!!

## Удаление данных
```
DELETE FROM table
WHERE condition
```
!!!

## Удаление по условию
```
DELETE
FROM
 link USING link_tmp
WHERE
 link.id = link_tmp.id;
```
!!!

## Удаление всех строк
```
DELETE
FROM
 link;
 ```
 !!!

## Можно так
```
DELETE
FROM
 link RETURNING *;
```
!!!

## Вопросы

!!!
