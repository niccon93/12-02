# Домашнее задание к занятию «Работа с данными (DDL/DML)»

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

```
sudo apt update
sudo apt install gnupg
wget -c https://dev.mysql.com/get/mysql-apt-config_0.8.24-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.24-1_all.deb
sudo apt update
sudo apt install -y mysql-server # задаём пароль 12345 для пользователя root в СУБД MySQL
sudo systemctl status mysql.service
```
![img](IMG/1.PNG)

1.2. Создайте учётную запись sys_temp. 

```
mysql -u root -p 
create user 'sys_temp'@'%' identified by '12345678';
exit
```

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

```
mysql -u root -p 
select User,Host from mysql.user;
exit
```
![img](IMG/1-1.PNG)

1.4. Дайте все права для пользователя sys_temp. 

```
mysql -u root -p 
grant ALL PRIVILEGES on *.* to 'sys_temp'@'%' with GRANT option;
flush privileges;
exit
```

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

```
mysql -u root -p 
show grants for 'sys_temp'@'%';
exit
```
![img](IMG/1-2.PNG)

1.6. Переподключитесь к базе данных от имени sys_temp.
Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

```
mysql -u sys_temp -p 
alter user 'sys_temp'@'%' IDENTIFIED with mysql_native_password by '12345678';
exit
```

1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

```
wget -c https://downloads.mysql.com/docs/sakila-db.zip
unzip sakila-db.zip
cd sakila-db
```

1.7. Восстановите дамп в базу данных.

```
mysql -u sys_temp -p
show databases;
create database SakilaDB;
show databases;
use SakilaDB;
show tables;
source sakila-schema.sql;
source sakila-data.sql;
show tables;
exit
```

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

```
mysql -u sys_temp -p
show databases;
use SakilaDB;
show tables;
exit
```
![img](IMG/1-3.PNG)


### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```

```
Название таблицы            | Название первичного ключа
actor                       |  actor_id
actor_info                  |  -
address                     |  address_id
category                    |  category_id
city                        |  city_id
country                     |  country_id
customer                    |  customer_id
customer_list               |   -
film                        |  film_id
film_actor                  |  actor_id, film_id
film_category               |  film_id, category_id
film_list                   |   -
film_text                   |   -
inventory                   |  inventory_id
language                    |  language_id
nicer_but_slower_film_list  |   -
payment                     |  payment_id
rental                      |  rental_id
sales_by_film_category      |   -
sales_by_store              |   -
staff                       |  staff_id
staff_list                  |   -
store                       |  store_id
```

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.

```
mysql -u root -p 
show grants for 'sys_temp'@'%';
grant ALL PRIVILEGES on SakilaDB.* to 'sys_temp'@'%';
revoke INSERT, UPDATE, DELETE on SakilaDB.* from 'sys_temp'@'%';
show grants for 'sys_temp'@'%';
flush privileges;
exit
```

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

```
mysql -u root -p 
show grants for 'sys_temp'@'%';
exit
```
![img](IMG/1-4.PNG)
