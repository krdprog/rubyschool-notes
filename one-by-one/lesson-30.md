## Урок 30

#### 5 шагов по созданию миграции:
1. подключаем БД
```ruby
set :database, "sqlite3.my_database.db"
```
2. создаём модель (класс)
```ruby
class Client < ActiveRecord::Base
end
```
3. создаём миграцию
```bash
rake db:create_migration NAME=create_clients
```
4. редактируем миграцию
```ruby
class CreateClients < ActiveRecord::Migration[5.2]
  def change
    create_table :clients do |t|
      t.text :name
      t.text :phone
      t.text :datestamp
      t.text :barber

      t.timestamps
    end
  end
end
```
5. делаем миграцию
```bash
rake db:migrate
```
Необходимо наличие всех гемов и Rakefile

#### Сохранение в БД через ActiveRecord

##### Ламерский способ:
```ruby
# + to app.rb

post '/order' do
  @name = params[:name]
  @phone = params[:phone]
  @datestamp = params[:datestamp]
  @barber = params[:barber]

  c = Client.new
  c.name = @name
  c.phone = @phone
  c.datestamp = @datestamp
  c.barber = @barber
  c.save

  erb :sent
end
```

##### Способ лучше:

```ruby
# + to app.rb
post '/order' do
  c = Client.new params[:client]
  c.save

  erb :sent
end
```

```ruby
# + to views/order.erb

<form action="/order" method="POST">

  <p><label for="name">Ваше имя:</label></p>
  <p><input type="text" name="client[name]" value=""></p>

  <p><label for="phone">Телефон:</label></p>
  <p><input type="text" name="client[phone]" value=""></p>

  <p><label for="datestamp">Выберите дату:</label></p>
  <p><input type="text" name="client[datestamp]" value=""></p>

  <p><label for="barber">Парикмахер:</label>
    <select name="client[barber]">
      <option value="" selected>Выбрать парикмахера...</option>

      <% @barbers.each do |barber| %>
      <option value="<%= barber.name %>"><%= barber.name %></option>
      <% end %>

    </select>
  </p>

  <br>
  <p><input type="submit" value="Отправить заявку"></p>

</form>
```
Т.е. вместо создания переменных из params. мы сократили код до пары строк:
```ruby
c = Contact.new params[:contact]
c.save
```
и в представлении:
```ruby
<p><input type="text" name="contact[name]" value=""></p>
<p><textarea rows="10" cols="45" name="contact[comment]"></textarea></p>
```
> Важное замечание: метод save для новых записей проводит валидацию, если всё правильно, то возвращает true иначе false

#### Настройка валидации:

Можно проверить пустое - не пустое, можно проверить длину

```ruby
# + to app.rb

class Client < ActiveRecord::Base
  validates :name, presence: true
  validates :phone, presence: true
  validates :datestamp, presence: true
  validates :barber, presence: true
end
```
Проверка в tux:
```bash
c = Client.new
c.valid?
c.errors.count
c.errors.messages
```
```ruby
# + to app.rb

post '/order' do
  c = Client.new params[:client]
  c.save

  if c.save
    erb "<p>Thank you!</p>"
  else
    erb "<p>Error</p>"
  end
end
```
или:
```ruby
# + to app.rb

post '/order' do
  c = Client.new params[:client]
  c.save

  if c.save
    erb "<p>Thank you!</p>"
  else
    @error = c.errors.full_messages.first
    erb :order
  end
end
```
```ruby
# + views/order.erb

<p style="color: red"><%= @error %></p>
```
Чтобы сохранять данные в поле формы после перезагрузки страницы при неправильном заполнении, надо сделать так:

```ruby
# + to app.rb

get '/order' do
  @c = Client.new

  erb :order
end

post '/order' do
  @c = Client.new params[:client]
  @c.save

  if @c.save
    erb "<p>Thank you!</p>"
  else
    @error = @c.errors.full_messages.first
    erb :order
  end
end
```

```ruby
# + to views/order.erb

<p><input type="text" name="client[name]" value="<%= @c.name %>"></p>
<p><input type="text" name="client[phone]" value="<%= @c.phone %>"></p>
<p><input type="text" name="client[datestamp]" value="<%= @c.datestamp %>"></p>
```
Для textarea:
```ruby
<p><textarea rows="10" cols="45" name="contact[comment]"><%= @c.comment %></textarea></p>
```

> Ссылка на полный проект Barbershop Sinatra with ActiveRecord:
> https://github.com/krdprog/barbershop-sinatra-with-activerecord

#### Популярные свойства валидации ActiveRecord:

> Ссылка:
> Active Record Validations — Ruby on Rails Guides
> https://guides.rubyonrails.org/active_record_validations.html

- length - length: { minimum: 3 } https://guides.rubyonrails.org/active_record_validations.html#length
- numericality: true - проверка, введены ли числа
- inclusion - https://guides.rubyonrails.org/active_record_validations.html#inclusion

> **Домашнее задание:** переписать блог с использованием ActiveRecord

#### Создадим отдельные страницы для каждого парикмахера:

```ruby
# + to app.rb

get '/barber/:id' do
  @barber = Barber.find(params[:id])

  erb :barber
end
```

```ruby
# + to views/index.erb

<h2>Список парикмахеров:</h2>

<ul>
<% @barbers.each do |barber| %>
  <li><a href="/barber/<%= barber.id %>"><%= barber.name %></a></li>
<% end %>
</ul>
```

```ruby
# + to views/barber.erb

<h2>Barber page</h2>

<p>Name: <%= @barber.name %></p>
```

> Find в ActiveRecord:
> https://guides.rubyonrails.org/active_record_querying.html

#### Показать записавшихся в обратном порядке (кто свежий - наверху):

```ruby
# + to app.rb

get '/clients' do
  @clients = Client.order('created_at DESC')

  erb :clients
end

get '/clients/:id' do
  @client = Client.find(params[:id])

  erb :client
end
```

```ruby
# + to views/clients.erb

<h2>Список записавшихся</h2>

<table border="1" cellpadding="10" cellpadding="0">
  <tr>
    <th>Name</th>
    <th>Phone</th>
    <th>Date</th>
    <th>Barber</th>
  </tr>

<% @clients.each do |client| %>
  <tr>
    <td><a href="/clients/<%= client.id %>"><%= client.name %></a></td>
    <td><%= client.phone %></td>
    <td><%= client.datestamp %></td>
    <td><%= client.barber %></td>
  </tr>
<% end %>
</table>
```

```ruby
# + to views/client.erb

<h2>Страница клиента:</h2>

<p><strong>Имя:</strong> <%= @client.name %></p>

<p><a href="/clients"><< назад к списку записавшихся</a></p>
```

##### В ActiveRecord есть штука, которая связывает автоматически посты и комментарии. Домашнее задание - найти это и написать движок блога.

> ActiveRecord "one-to-many" (отношение сущностей)
> https://guides.rubyonrails.org/association_basics.html

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |