## Урок 22

#### Решение, чтобы не перезапускать Sinatra

- https://rubygems.org/gems/sinatra-reloader - gem sinatra reloader
- https://rubygems.org/gems/sinatra-contrib - gem sinatra contrib (в его состав входит sinatra-reloader, возможно в некоторых случаях надо попробовать поставить его).

Надо установить гем sinatra-reloader:
```ruby
gem install sinatra-reloader
```
или установить sinatra-contrib:
```ruby
gem install sinatra-contrib
```
или так:
```ruby
sudo apt install ruby-sinatra-contrib
```
и, добавляем в свои app.rb строчку:
```ruby
require 'sinatra/reloader'
```

#### Про yield в views/layout.erb

app.rb:
```ruby
require 'sinatra'

get '/' do
  erb "Hello all Ruby-programmers!"
end
```
views/layout.erb:
```ruby
<h1>Some text</h1>
<%= yield %>
```
Информация из app.rb будет помещена в yield в views/layout.erb

В файле views/layout.erb можно сделать основной каркас страницы, разместить yield и уже из других erb подгружать информацию.

app.rb
```ruby
get '/foo' do
  erb :foo
end
```
views/layout.erb
```ruby
<p>This block for all site</p>
<p>Block navigation</p>
<%= yield %>
```
views/foo.erb
```ruby
<h1>Foo Header</h1>
<p>Text for Foo.</p>
```

#### Напомним, как работает yield:
```ruby
def show_me_text
  puts "<body>"
  yield
  puts "</body>"
end

show_me_text { puts "Foo!" }

# OR:

show_me_text do
  puts "Foo!"
end

# puts "Foo!" вставится в место, где указан yield
```
#### Задание:
В Babershop добавить возможность выбора парикмахера (через select).

#### Решение:
В views/index.erb добавить часть кода:
```ruby
	<label for="baber">Выбор парикмахера</label>
	<select name="baber">
		<option value="none" selected>Выберите парикмахера...</option>
		<option value="Петрович">Петрович</option>
		<option value="Макарыч">Макарыч</option>
		<option value="Федорыч">Федорыч</option>
	</select>
```
А, в babershop-2.rb добавить и исправить код:
```ruby
post '/' do
	# user_name, phone, date_time
	@user_name = params[:user_name]
	@phone = params[:phone]
	@date_time = params[:date_time]
	@baber = params[:baber]

	@title = "Thank you!"
	@message = "Уважаемый #{@user_name}, мы ждём вас #{@date_time} у выбранного парикмахера #{@baber}."

  # запишем в файл то, что ввёл клиент
	f = File.open 'users.txt', 'a'
	f.write "User: #{@user_name}, phone: #{@phone}, date and time: #{@date_time}. Baber: #{@baber}.\n"
	f.close

	erb :message
end
```
Здесь мы добавили код:
```ruby
@baber = params[:baber]
```
плюс добавили в @message и в код записи в в файл f.write 

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md