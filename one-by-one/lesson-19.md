### Урок 19

#### Sinatra

```sh
gem install sinatra
```

first program:
```ruby
require "sinatra"

get '/' do
	"Hello!"
end
```
Создадим каталог views
```sh
mkdir views
```
Создадим файл views/index.erb
```sh
touch views/index.erb
```
Поправим наш файл:
```ruby
require "sinatra"

get '/' do
	erb :index
end
```

#### Пример с POST:

app.rb
```ruby
require "sinatra"

get '/' do
	erb :index
end

post '/'do
	@login = params[:login]
	erb :index
end
```
views/index.erb
```ruby
<h1>Foo Bar Baz</h1>

<p>You typed: <%= @login %></p>

<form action="/" method="POST">
	<input type="text" name="login">
	<input type="password" name="pass">
	<input type="submit">
</form>
```

#### Расширим:

app.rb
```ruby
require "sinatra"

get '/' do
	erb :index
end

post '/' do
	@login = params[:login]
	@password = params[:pass]

	if @login == 'admin' && @password == '12345'
	  erb :welcome
	else
      @if_stop = "STOP!"
      erb :index
	end
end

get '/contacts' do
	"Contacts: +7 000 000-00-00"
end
```
views/index.erb
```ruby
<h1>Foo Bar Baz</h1>

<p>You typed: <%= @login %></p>

<p><%= @if_stop %></p>

<form action="/" method="POST">
	<input type="text" name="login">
	<input type="password" name="pass">
	<input type="submit">
</form>
```
views/welcome.erb
```ruby
<h1>Welcome!</h1>
<p>You login in system.</p>

<p><a href="/contacts">Contacts</a></p>
```
> Отдельная область программирования scraping - парсинг информации

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md