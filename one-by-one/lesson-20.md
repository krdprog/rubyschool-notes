## Урок 20

app.rb
```ruby
require "sinatra"

get '/' do
	"Hello Foo Bar"
end

get '/contacts' do
	@title = "Contacts"
	@message = "Hello all! Phone me now."
	erb :message
end

get '/faq' do
	@title = "FAQ about our products"
	@message = "It`s FAQ"
	erb :message
end

get '/none-page' do
	under_construction
end

def under_construction
	@title = "Under Construction"
	@message = "This page is under construction."
	erb :message
end
```
views/message.erb
```ruby
<h1><%= @title %></h1>

<p><%= @message %></p>
```

Я пользуюсь git в командной строке, но можно использовать ungit (npm-пакет).

#### Babershop.rb

Listing babershop.rb
```ruby
require 'sinatra'

get '/' do
	erb :index
end

# спросим Имя, номер телефона и дату, когда придёт клиент.
post '/' do
	# user_name, phone, date_time
	@user_name = params[:user_name]
	@phone = params[:phone]
	@date_time = params[:date_time]

	@title = "Thank you!"
	@message = "Уважаемый #{@user_name}, мы ждём вас #{@date_time}"

  # запишем в файл то, что ввёл клиент
 	f = File.open 'users.txt', 'a'
	f.write "User: #{@user_name}, phone: #{@phone}, date and time: #{@date_time}.\n"
	f.close

	erb :message
end
```

Listing views/index.erb
```ruby
<h1>Babershop</h1>

<form action="/" method="POST">
	Your name:<br> <input type="text" name="user_name"><br>
	Your phone:<br> <input type="text" name="phone"><br>
	Date and time:<br> <input type="text" name="date_time"><br>

	<input type="submit">
</form>
```

Listing views/message.erb
```ruby
<h1><%= @title %></h1>

<p><%= @message %></p>
```

#### Задание:

В программу babershop.rb добавить зону /admin где по паролю будет выдаваться список тех, кто записался (из users.txt)

Подсказка: ищи sinatra text file на stackoverflow

#### Варианты чтения из файла в синатру:
1:
```ruby
get '/result' do
  send_file 'users.txt'
end
```
2:
```ruby
get '/result' do
  @file = File.open("users.txt","r")
  erb :result
  # @file.close - с этим у меня не работает, перенёс в erb
end
```
Listing views/result.erb:
```ruby
<h2>RESULT:</h2>
<% @file.each_line do |line| %>
  <%= line %>
<% end %>

<%= @file.close %>
```
или:
```ruby
<h2>RESULT:</h2>
<%= @file.read %>
<%= @file.close %>
```

#### Решение:

babershop-2.rb
```ruby
require 'sinatra'

get '/' do
	erb :index
end

# спросим Имя, номер телефона и дату, когда придёт клиент.
post '/' do
	# user_name, phone, date_time
	@user_name = params[:user_name]
	@phone = params[:phone]
	@date_time = params[:date_time]

	@title = "Thank you!"
	@message = "Уважаемый #{@user_name}, мы ждём вас #{@date_time}"

  # запишем в файл то, что ввёл клиент
	f = File.open 'users.txt', 'a'
	f.write "User: #{@user_name}, phone: #{@phone}, date and time: #{@date_time}.\n"
	f.close

	erb :message
end

# Добавить зону /admin где по паролю будет выдаваться список тех, кто записался (из users.txt)

# sinatra text file sso
get '/admin' do
  erb :admin
end

post '/admin' do
	@login = params[:login]
	@password = params[:password]

	# проверим логин и пароль, и пускаем внутрь или нет:
	if @login == 'admin' && @password == 'krdprog'
  	@file = File.open("./users.txt","r")
  	erb :watch_result
	  # @file.close - должно быть, но тогда не работает. указал в erb
	else
		@report = '<p>Доступ запрещён! Неправильный логин или пароль.</p>'
		erb :admin
	end
end
```
views/index.erb
```ruby
<h1>Babershop 2</h1>

<form action="/" method="POST">
	Your name:<br> <input type="text" name="user_name"><br>
	Your phone:<br> <input type="text" name="phone"><br>
	Date and time:<br> <input type="text" name="date_time"><br>

	<input type="submit">
</form>
```

views/admin.erb
```ruby
<h1>Admin Panel</h1>

<form action="/admin" method="POST">
	Login:<br> <input type="text" name="login"><br>
	Password:<br> <input type="password" name="password"><br>

	<input type="submit">
</form>

<%= @report %>
```

views/message.erb
```ruby
<h1><%= @title %></h1>

<p><%= @message %></p>
```

views/watch_result.erb
```ruby
<h1>Admin Panel (Watch Result)</h1>


<table border="1">
<% @file.each_line do |line| %>
  <tr><td><%= line %></td></tr>
<% end %>
</table>

<%= @file.close %>
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md