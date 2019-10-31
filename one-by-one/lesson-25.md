## Урок 25

#### Базы данных - gem sqlite3

bash:
```bash
gem install sqlite3
```
> sqlite - https://sqlite.org/index.html

**NOTE:** При первой установке и настройке, помимо установки гема, возможно потребуется дополнительно установить libsqlite3-dev по следующей команде (перед установкой гема):

```bash
sudo apt-get install libsqlite3-dev
```

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
> SQLite Browser https://sqlitebrowser.org/dl/

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

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |