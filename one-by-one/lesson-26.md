## Урок 26

Как сохранять введённые данные в поле selected (html):
```ruby
<option value="Опция 1"<%= @option == 'Опция 1' ? ' selected' %>>Опция 1</option>
```
app.rb
```ruby
require 'sqlite3'

db = SQLite3::Database.new 'base.sqlite'

```
В SQLite использовать тип TEXT, поправка к предыдущему уроку.

#### SQL:

##### Такой способ записи будем применять для создания базы данных и таблиц в ней при инициализации приложения:
```sql
CREATE TABLE IF NOT EXISTS "Users" ("Id" INTEGER PRIMARY KEY AUTOINCREMENT, "Username" TEXT, "Phone" TEXT);
```
Удалить таблицу:

```sql
DROP TABLE Users;
```
>  Ссылка: Шпаргалка по SQLite3 (русская версия)
>  https://github.com/krdprog/sqlite-3-rus-howto


> Базу данных не принято коммитить в репозиторий и мешать с исходным кодом. Базу данных надо добавлять в .gitignore

##### В Sinatra есть специальная команда, которая запускается при инициализации приложения:
```ruby
configure do
  # some code
end
```
##### Создадим базу данных при инициализации приложения:

При этом у нас подключены гемы:

```ruby
require 'sinatra'
require 'sqlite3'
```

```ruby
configure do
  @db = SQLite3::Database.new 'base.db'

  @db.execute 'CREATE TABLE IF NOT EXISTS "Messages"
    (
      "id" INTEGER PRIMARY KEY AUTOINCREMENT,
      "username" TEXT,
      "phone" TEXT,
      "email" TEXT,
      "option" TEXT,
      "comment" TEXT
    )'
  # db.close (надо?)
end
```
или в одну строку, но менее читаемо:
```ruby
configure do
  @db = SQLite3::Database.new 'base.db'

  @db.execute 'CREATE TABLE IF NOT EXISTS "Messages" ("id" INTEGER PRIMARY KEY AUTOINCREMENT, "username" TEXT, "phone" TEXT, "email" TEXT, "option" TEXT, "comment" TEXT)'
  # @db.close (надо?)
end
```
> Ссылка: Escaping Strings For Ruby SQLite Insert
> https://stackoverflow.com/questions/9614236/escaping-strings-for-ruby-sqlite-insert

> Prepare:
> https://www.rubydoc.info/github/luislavena/sqlite3-ruby/SQLite3/Database#prepare-instance_method

> Про вставку данных:
> https://stackoverflow.com/questions/13462112/inserting-ruby-string-into-sqlite


> Документация:
> https://www.rubydoc.info/github/luislavena/sqlite3-ruby
>
> https://www.rubydoc.info/github/luislavena/sqlite3-ruby/SQLite3/Database

#### Вставка данных в БД в Sinatra app:

Переменная @db не сработала, сделали через db

```ruby
require 'sinatra'
require 'sqlite3'

def get_db
  return SQLite3::Database.new 'base.db'
end

configure do
  db = get_db
  db.execute 'CREATE TABLE IF NOT EXISTS "Messages"
    (
      "id" INTEGER PRIMARY KEY AUTOINCREMENT,
      "username" TEXT,
      "phone" TEXT,
      "email" TEXT,
      "option" TEXT,
      "comment" TEXT
    )'
  db.close
end

def save_form_data_to_database
  db = get_db
  db.execute 'INSERT INTO Messages (username, phone, email, option, comment)
  VALUES (?, ?, ?, ?, ?)', [@username, @phone, @email, @option, @comment]
  db.close
end

get '/' do
  @title = "Форма заявки для Sinatra (Ruby)"
  erb :index
end

post '/' do
  @username = params[:username]
  @phone = params[:phone]
  @email = params[:email]
  @option = params[:option]
  @comment = params[:comment]

  save_form_data_to_database

  @title = "Спасибо, ваше сообщение отправлено"
  erb :sent
end
```
> Полная работающая версия формы со всеми файлами тут:
> https://github.com/krdprog/contact-form-for-sinatra-ruby-rus

К полю выбора даты можно подключить плагин:

> Плагин для выбора даты (js)
> https://github.com/xdan/datetimepicker

#### Вывод результатов (данных) из базы данных:

Чтобы выводить результаты в виде хеша, надо добавить:
```ruby
db.results_as_hash = true
```
Добавим в ранее созданный метод и переделаем app.rb:
```ruby
def get_db
  db = SQLite3::Database.new 'base.db'
  db.results_as_hash = true
  return db
end
```

#### Домашнее задание:
1. сделать страницу показа результата из базы данных используя запрос

```sql
SELECT * FROM Users ORDER BY id DESC
```

> Ссылка SQLite ORDER BY:
> http://www.tutorialspoint.com/sqlite/sqlite_order_by.htm

2. В configure сделать дополнительную таблицу Barbers со списком парикмахеров. Загружать список парикмахеров в configure (вставка в таблицу 1 раз).

3. Загрузить данные из таблицы Barbers в форму в выпадающий список select.

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |