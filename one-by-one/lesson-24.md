## Урок 24

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

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |