# Database_Administration_for_DevOps_Engineers_HW_2
## VADIM TSVETKOV

«SQL»

**Задание 1.**
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/1.1.jpg)
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/1.2.jpg)
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/1.3.jpg)

**Задание 2.**
Подключаемся к ранее созданной БД и созданным юзером
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/2.1.jpg)
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/2.2.jpg)
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/2.3.jpg)
Итоговый список БД после выполнения пунктов выше
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/2.4.jpg)
Описание таблиц (describe);
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/2.5.jpg)
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/2.6.jpg)
SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
select distinct grantee from information_schema.role_table_grants where table_catalog = 'test_bd';
Cписок пользователей с правами над таблицами test_bd
![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/2.7.jpg)

**Задание 3.**
INSERT INTO orders (name, price) VALUES
('Шоколад', 10),
('Принтер', 3000),
('Книга', 500),
('Монитор', 7000),
('Гитара', 4000);

INSERT INTO clients (last_name, country) VALUES
('Иванов Иван Иванович', 'USA'),
('Петров Петр Петрович', 'Canada'),
('Иоганн Себастьян Бах', 'Japan'),
('Ронни Джеймс Дио', 'Russia'),
('Ritchie Blackmore', 'Russia');

SELECT COUNT(*) FROM orders;
SELECT COUNT(*) FROM clients;

![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/3.1.jpg)

**Задание 4.**
Приведите SQL-запросы для выполнения этих операций
update  clients set booking = 3 where id = 1;
update  clients set booking = 4 where id = 2;
update  clients set booking = 5 where id = 3;

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса

SELECT clients.lastname, clients.country, orders.name AS заказ
FROM clients
INNER JOIN orders ON clients.booking = booking
WHERE clients.booking IS NOT NULL;

![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/4.1.jpg)

**Задание 5.**
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).

Приведите получившийся результат и объясните, что значат полученные значения

explain select lastname from clients where booking is not null;

![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/5.1.jpg)

cost указывает время затраченое на получение первой записи и всем записей, rows - кол-во проверяемых строк, width - средний размер строки в байтах, указание применение фильтра/

**Задание 6.**

Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).

docker exec -it postgres12 bash
pg_dump -h -U test_admin_user -d test_bd > /var/backup/test_bd.sql

Остановите контейнер с PostgreSQL, но не удаляйте volumes

![img](https://github.com/vadimtsvetkov/Database_Administration_for_DevOps_Engineers_HW_2/blob/main/6.1.jpg)

Поднимите новый пустой контейнер с PostgreSQL

sudo docker run -d \
  --name test_db1 \
  -v /var/backup:/backups \
  -e POSTGRES_DB=test_bd \
  -e POSTGRES_USER=test_admin_user \
  -e POSTGRES_PASSWORD=admin \
  postgres:12
  
  Восстановите БД test_db в новом контейнере
  
  psql -U test_admin_user -d test_bd  > /var/backups/test_bd.sql
