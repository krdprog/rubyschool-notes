## Конспект RubySchool.us [4]

### Урок 26

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

### Урок 27

> Отделить логику от представления.

Синтаксис в erb, будет выполнено, но ничего не будет выводиться:
```ruby
<% %>
```
Будет выводиться в месте, где стоит:
```ruby
<%= %>
```
#### Отображение на веб-странице данных из базы данных:

Добавить в app.rb
```ruby
# Method connect with database
def get_db
	@db = SQLite3::Database.new 'base.db'
	@db.results_as_hash = true
	return @db
end

# Show result in admin panel
get '/admin/show' do
	get_db

	@results = @db.execute 'SELECT * FROM Messages ORDER BY id DESC'
	@db.close

	erb :show
end
```
views/show.erb
```ruby
<table border="1">
	<tr>
		<th>Имя</th>
		<th>Телефон</th>
		<th>E-mail</th>
		<th>Опция</th>
		<th>Комментарий</th>
	</tr>

<% @results.each do |row| %>

	<tr>
		<td><%= row['username'] %></td>
		<td><%= row['phone'] %></td>
		<td><%= row['email'] %></td>
		<td><%= row['option'] %></td>
		<td><%= row['comment'] %></td>
	</tr>

<% end %>

</table>
```
Покрасим в красный цвет первую запись:

```ruby
<%= "style='background-color: red; color: white;'" if row == @results.first %>
```

erb:
```ruby
<% @results.each do |row| %>

	<tr <%= "style='background-color: red; color: white;'" if row == @results.first %>>
		<td><%= row['username'] %></td>
		<td><%= row['phone'] %></td>
		<td><%= row['email'] %></td>
		<td><%= row['option'] %></td>
		<td><%= row['comment'] %></td>
	</tr>

<% end %>
```
#### Решение задания: 
> В configure сделать дополнительную таблицу Barbers со списком парикмахеров. Загружать список парикмахеров в configure (вставка в таблицу 1 раз) - сделаю для контактной формы для поля Options:

```ruby
# Method validation data Options table in database
def is_option_exists? base, param
	base.execute('SELECT * FROM Options WHERE option=?', [param]).length > 0
end

# Method add data to table in database
def seed_db base, options
	options.each do |option|
		if !is_option_exists? @db, option
			base.execute 'INSERT INTO Options (option) VALUES (?)', [option]
		end
	end
end
```

```ruby
# Configure application
configure do
	get_db
	# create table Messages in database
	@db.execute 'CREATE TABLE IF NOT EXISTS "Messages"
	  (
		  "id" INTEGER PRIMARY KEY AUTOINCREMENT,
		  "username" TEXT,
		  "phone" TEXT,
		  "email" TEXT,
		  "option" TEXT,
		  "comment" TEXT
		)'

	# create table Options in database
	@db.execute 'CREATE TABLE IF NOT EXISTS "Options"
	  (
		  "id" INTEGER PRIMARY KEY AUTOINCREMENT,
		  "option" TEXT
		)'

	# add data to table
	seed_db @db, ['Foo', 'Faa', 'Moo', 'Zoo', 'Faz', 'Maz', 'Kraz']

	@db.close
end
```
seed_db - seed устоявшееся выражение - наполнить

#### Понятие миграция (база данных).

Механизмы миграции, чтобы версия базы данных всегда совпадала с версией кода.

to keep up to date - держать в актуальном состоянии

Откат БД вместе с программным кодом.

Дальше будем проходить наполнение и миграции. База наполняется из програмного кода.

#### Синтаксис before (в sinatra) - исполняет код перед запросом - будет доступно во всех представлениях

```ruby
before do
  # some code
end
```

#### Как загружать варианты в select формы из базы данных:
В app.rb добавим часть кода:
```ruby
# Index page with form
get '/' do

	# Write to array data from database table Options
	get_db
	@options = @db.execute 'SELECT * FROM Options'
	@db.close

	@title = "Форма заявки для Sinatra (Ruby)"
	erb :index
end
```

в views/index.erb в форме заменим статические данные на :
```ruby
	<p>
		<select name="option">
			<option value="" selected>Выбрать опцию...</option>
			<% @options.each do |item| %>
			<option value="<%= item['option'] %>"><%= item['option'] %></option>
			<% end %>
		</select>
	</p>
```

Вставка, чтобы сделать select - selected
```ruby
<option <%= @barber == item['name'] ? 'selected' : '' %>><%= item['name'] %></option>
```

configure - при запуске программы
before - при каждом обращении к программе

### Урок 28

> Мой вариант Sinatra Blog:
> https://github.com/krdprog/sinatra-blog
