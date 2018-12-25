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

Следующий урок: https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md