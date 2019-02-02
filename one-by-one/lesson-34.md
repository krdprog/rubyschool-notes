## Урок 34

#### x ||= y

Это сокращённо:
```ruby
x = x || y
```
Что означает:
```ruby
x || y
```
|| - логическое ИЛИ

```ruby
if x == 1 || x == 2
```
Срабатывает первое условие, если не срабатывает, берёт второе условие:
```ruby
nil || 4 #=> 4
false || 2 #=> 2
123 || 2 #=> 123

x = x || 4
```

```ruby
x = false
x = x || 2
puts x
#=> 2

x = true
x = x || 2
puts x
#=> true

x = 5
x = x || 2 # это мы можем заменить на x ||= 2
puts x
#=> 5

x = 5
x ||= 2
puts x
#=> 5
```
#### x ||= y используется для установки значения по умолчанию

#### Продолжаем PizzaShop:

Напишем подсчёт количества товаров в корзине на js:

```js
// + to public/js/main.js

function cart_get_number_of_items()
{
  var cnt = 0;

  for (var i = 0; i < window.localStorage.length; i++)
  {
    var key = window.localStorage.key(i); // получаем ключ
    var value = window.localStorage.getItem(key); // получаем значение

    if(key.indexOf('product_') == 0)
    {
      cnt = cnt + value * 1;
    }
  }

  return cnt;
}
```
Проверим в консоли firefox:
```txt
cart_get_number_of_items()
```

```ruby
# + to views/layout.erb
<form action="/cart" method="POST">
  <input type="submit" value="Корзина (0 шт.)">
</form>
```

#### ИТОГО:
```ruby
# + to views/layout.erb

<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title></title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

<p><a href="/">Главная страница</a> | <a href="/products">Наша продукция</a>

  <form action="/cart" method="POST">
    <input type="hidden" name="orders" id="orders_input">
    <input type="submit" id="orders_button">
  </form>
</p>

<%= yield %>


  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
  <script src="/js/main.js"></script>

  <script>
    $(function() {
      update_orders_input();
      update_orders_button();
    });
  </script>

</body>
</html>
```
```js
// + to public/js/main.js

function add_to_cart(id)
{
  var key = 'product_' + id;

  var x = window.localStorage.getItem(key);
  x = x * 1 + 1;
  window.localStorage.setItem(key, x);

  update_orders_input();
  update_orders_button();
}


function cart_get_number_of_items()
{
  var cnt = 0;

  for (var i = 0; i < window.localStorage.length; i++)
  {
    var key = window.localStorage.key(i); // получаем ключ
    var value = window.localStorage.getItem(key); // получаем значение

    if(key.indexOf('product_') == 0)
    {
      cnt = cnt + value * 1;
    }
  }

  return cnt;
}


function update_orders_input()
{
  orders = cart_get_orders();
  $('#orders_input').val(orders);
}


function cart_get_orders()
{
  var orders = '';

  for (var i = 0; i < window.localStorage.length; i++)
  {
    var key = window.localStorage.key(i); // получаем ключ
    var value = window.localStorage.getItem(key); // получаем значение

    if(key.indexOf('product_') == 0)
    {
      orders = orders + key + '=' + value + ',';
    }
  }

  return orders;
}


function update_orders_button()
{
  var text = 'Корзина (' + cart_get_number_of_items() + ' шт.)';
  $('#orders_button').val(text);
}
```

#### Домашнее задание:
- на странице /cart вывести в виде таблицы список продуктов в корзине и их количество
- на странице /cart сделать так, чтобы форма сабмитилась по адресу /order и чтобы в базу данных заносился заказ: имя, телефон, адрес доставки, список купленных товаров (в виде текстового поля).

### Ruby on Rails

Я устанавливаю ruby через RVM

Посмотрим доступные версии Rails:

```bash
gem search '^rails$' --all
```
Чтобы установить конкретную версию, введите (вместо rails_version - номер версии):
```bash
gem install rails -v rails_version
```
С помощью gemset-ов можно использовать вместе разные версии Rails и Ruby. Это делается с помощью команды gem.

```ruby
rvm gemset create gemset_name # create a gemset
rvm ruby_version@gemset_name  # specify Ruby version and our new gemset
```
Gemset-ы позволяют создавать полнофункциональные окружения для gem-ов, а также настраивать неограниченное количество окружений для каждой версии Ruby.

> [GitHub - DefactoSoftware/Hours: Time registration that doesn't suck](https://github.com/DefactoSoftware/Hours)

#### Rails-приложение может запускаться в 3 типах окружения:
- development
- test
- production

#### Создадим новое рейлс-приложение:
```bash
rails new blog
```
Последовательность запуска rails (для справки):
```txt
boot.rb -> rails -> environment.rb -> development.rb (test.rb or production.rb)
```
Запуск приложения:

```bash
rails s
```
Если не запустится, установи nodejs:
```bash
sudo apt-get install nodejs
```
Если всё ок, приложение запустится и можно открывать в браузере: http://localhost:3000/

Обновлять bundle можно командой:
```bash
bundle update
```
### MVC
Model, View, Controller

Controller - отвечает за работу с какой-либо сущностью

#### Создадим контроллер:
```bash
rails generate controller home index
```
Найдём файл /app/controllers/home_controller.rb

Найдём файл /app/views/home/index.html.erb

Откроем в браузере: http://localhost:3000/home/index

> Обычно /home/index создаётся для главной страницы сайта
> Надо открыть /config/routes.rb и прописать:

```ruby
Rails.application.routes.draw do
  get '/' => 'home#index'
end
```

> Изучить: Rails Routing from the Outside In — Ruby on Rails Guides
> http://guides.rubyonrails.org/routing.html

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md