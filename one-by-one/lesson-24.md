### Урок 24

#### Валидация - проверка параметров

##### Вариант 1:
app.rb
```ruby
	if @user_name == ''
		@error = 'Ошибка: Введите имя!'
		return erb :index
	end
```
views/index.erb
```ruby
<p><strong><%= @error %></strong></p>
```
> Атомарность коммитов.

Возврат view вынесем отдельно:
```ruby
if @error != ''
  return erb :index
end
```
Для проверки валидации можно использовать хеши:

```ruby
# хеш для валидации параметров
hh = { :user_name => 'Введите имя',
  :phone => 'Введите телефон',
  :date_time => 'Введите дату и время' }

# для каждой пары ключ-значение
hh.each do |key, value|
  # если параметр пустой
  if params[key] == ''
    # переменной @error присвоить value из хеша hh
    @error = hh[key]
    
    # вернуть представление
    return erb :index
  end
end
```
##### Как сделать, чтобы введённое значение оставалось в поле после вывода ошибки валидации:

Добавим атрибут value в файле views/index.erb:
```ruby
Your name:<br> <input type="text" name="user_name" value="<%= @user_name %>">
```
В итоге получим улучшенную версию views/index.erb
```ruby
<h1>Babershop</h1>

<p><strong><%= @error %></strong></p>

<form action="/" method="POST">
	Your name:<br> <input type="text" name="user_name" value="<%= @user_name %>"><br>
	Your phone:<br> <input type="text" name="phone" value="<%= @phone %>"><br>
	Date and time:<br> <input type="text" name="date_time" value="<%= @date_time %>"><br>

	<br><br>
	<label for="baber">Выбор парикмахера</label>
	<select name="baber">
		<option value="none" selected>Выберите парикмахера...</option>
		<option value="Петрович">Петрович</option>
		<option value="Макарыч">Макарыч</option>
		<option value="Федорыч">Федорыч</option>
	</select>
	<br><br>

	<input type="submit">
</form>
```
Ещё можно сделать так (будет выводить через запятую пустые поля):

Edit babershop-2.rb:
```ruby
	# хеш для валидации параметров
	hh = { :user_name => 'Введите имя',
      :phone => 'Введите телефон', 
      :date_time => 'Введите дату и время' }

	@error = hh.select {|key,_| params[key] == ''}.values.join(", ")

	if @error != ''
		return erb :index
	end
```
Вынесем это решение в отдельный метод (сделать самостоятельно).

#### Домашнее задание:
Сделать отправку данных, которые введены в форму на электронную почту. Через гем Pony. Описание тут: https://stackoverflow.com/questions/2068148/contact-form-in-ruby-sinatra-and-haml

Главное, чтобы на gmail или другом почтовом сервере был открыт 587 порт.

> gem pony: https://github.com/benprew/pony/
> Документация gem Pony: https://www.rubydoc.info/gems/pony/1.12