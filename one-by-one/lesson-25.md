### Урок 25

#### Базы данных - gem sqlite3

bash:
```bash
gem install sqlite3
```
> sqlite - https://sqlite.org/index.html

Язык запросов SQL

```bash
sqlite3 --version
```

Создать базу данных в терминале:

```bash
sqlite3 my_base.sqlite
```
#### Немного SQL:
> Пройти упражнения на тренажёре SQL is Hard: http://www.sqlishard.com/

Выбрать все колонки для таблицы:
```sql
SELECT * FROM Customers
```
Выбрать определённые колонки:
```sql
SELECT Id, FirstName FROM Customers
```
или в другом порядке:
```sql
SELECT FirstName, Id FROM Customers
```
WHERE:
```sql
SELECT * FROM Customers WHERE id = 5
```
WHERE conditions:
```sql
SELECT * FROM Orders WHERE OrderTotal BETWEEN 100 AND 200
```
```sql
SELECT * FROM Orders WHERE DeliveryTime < '2013-06-20'
```
LIKE (поиск):
```sql
SELECT * FROM Customers WHERE LastName LIKE 'A%'
```
OR:
```sql
продолжение http://www.sqlishard.com/Exercise#/exercises/SELECT/S2.4
```
#### Инструмент:
> Addon Firefox: https://addons.mozilla.org/ru/firefox/addon/sqlite-manager (установить, перезагрузить браузер, потом нажать Alt - Инструменты - SQLite Manager, или вынести в боковое меню).

- создадим базу данных test.sqlite
- создадим таблицу Cars со столбцами:
-- Id (INTEGER) - первичный ключ, автоинкремент: INTEGER PRIMARY KEY AUTOINCREMENT;
-- Name (VARCHAR)
-- Price (INTEGER)

```sql
CREATE TABLE "Cars" ("Id" INTEGER PRIMARY KEY AUTOINCREMENT, "Name" VARCHAR, "Price" INTEGER)
```
Пример запроса:
```sql
SELECT * FROM Cars WHERE Price > 1000
```
Создание - INSERT:
```sql
INSERT INTO Cars (Id, Name, Price) VALUES (1, 'BMW', 10000)
```
А, с автоинкрементом:
```sql
INSERT INTO Cars (Name, Price) VALUES ('Audi', 3000)
```
#### Откроем базу через терминал:
```bash
sqlite3 test.sqlite
```
Список таблиц:
```bash
.tables
```
Сделаем запрос (в конце запроса точка с запятой, иначе на другую строку переходит вод запроса):
```sql
SELECT * FROM Cars
```
Фишка .mode column
```bash
.mode column
```
Включить заголовки:
```bash
.headers on
```
Добавить:
```sql
INSERT INTO Cars (Name, Price) VALUES ('Foo', 6743)
```
Выйти:
```bash
.exit
```
#### Как обращаться к базе данных из программы:

app.rb
```ruby
require 'sqlite3'

# подключим базу данных (которую делали выше)
db = SQLite3::Database.new 'test.sqlite'

# добавим новую информацию в базу данных
db.execute "INSERT INTO Cars (Name, Price) VALUES ('Jaguar', 7000000)"

# прочитаем данные из базы
db.execute "SELECT * FROM Cars" do |car|
	puts car
	puts "======"
end

db.close
```
Обратить внимание на тему SQL Injection
```sql
'DROP TABLE Cars -- 
```
Это нужно фильтровать. Как? Будет ниже.

#### Домашнее задание:

##### 1. Создать для BaberShop бд с таблицами:

Users:

Id - идентификатор, primary key, автоинкремент, тип INTEGER

Name - Varchar

Phone - Varchar

DateStamp - Varchar

Baber - Varchar

Color - Varchar

Contacts:

Id - идентификатор, primary key, автоинкремент, тип INTEGER

Email - Varchar

Message - Varchar

##### 2. Добавить через терминал и sqlite3 несколько записей в таблицу Users и Contacts