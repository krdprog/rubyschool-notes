### Урок 33

#### Разбор вопросов (2):

> Изучить: 15 Questions to Ask During a Ruby Interview · GitHub
> https://gist.github.com/krdprog/64a463de21fe77a8946019fde6662d67

- три способа вызвать метод в руби:
```ruby
# 1
object = Object.new
puts object.object_id

# 2
puts object.send(:object_id)

# 3
puts object.method(:object_id).call
```

- оператор ||=

> [operators - What does \||= (or-equals) mean in Ruby? - Stack Overflow](https://stackoverflow.com/questions/995593/what-does-or-equals-mean-in-ruby#14697343)

> [ruby - Что делает оператор «\|| =» в рубине?](https://stackoverrun.com/ru/q/2545320)

- Что такое self? self всегда указывает на текущий объект. Может быть вызван без создания объекта.

```ruby
class WhatIsSelf
  def test
    puts "At the instance level, self is #{self}"
  end

  def self.test
    puts "At the class level, self is #{self}"
  end
end

WhatIsSelf.test
 #=> At the class level, self is WhatIsSelf

WhatIsSelf.new.test
 #=> At the instance level, self is #<WhatIsSelf:0x28190>
```
> [15 Questions to Ask During a Ruby Interview · GitHub](https://gist.github.com/krdprog/64a463de21fe77a8946019fde6662d67#what-does-self-mean)

- Что такое Proc? Процедура. Три типа:
1. анонимные методы (функции без имени)
2. lambda
3. блок

```ruby
# wants a proc, a lambda, AND a block
def three_ways(proc, lambda, &block)
  proc.call
  lambda.call
  yield # like block.call
  puts "#{proc.inspect} #{lambda.inspect} #{block.inspect}"
end

anonymous = Proc.new { puts "I'm a Proc for sure." }
nameless  = lambda { puts "But what about me?" }

three_ways(anonymous, nameless) do
  puts "I'm a block, but could it be???"
end
 #=> I'm a Proc for sure.
 #=> But what about me?
 #=> I'm a block, but could it be???
 #=> #<Proc:0x00089d64> #<Proc:0x00089c74> #<Proc:0x00089b34>
```

#### Продолжение PizzaShop

- нам надо создать корзину, но чтобы при закрытии браузера данные сохранялись.

Помежуточная проверка работает ли кнопка:
```html
<p><button onclick="alert('hello')">Добавить в корзину</button></p>
```

##### Number 2:

js:
```js
function add_to_cart() {
    alert('hello all!');
}
```
html:
```html
<p><button onclick="add_to_cart()">Добавить в корзину</button></p>
```

> Плохие программисты беспокоятся о коде. Хорошие программисты беспокоятся о структурах данных и их взаимодействии (Л. Торвальдс)

```ruby
# + to app.rb

require 'sinatra'
require 'sinatra/reloader'
require 'sinatra/activerecord'

set :database, "sqlite3:database.db"

class Product < ActiveRecord::Base
end

get '/' do
    erb :index
end

get '/products' do
    @products = Product.all
    erb :products
end
```

```ruby
# + to views/products.erb

<h1>Наша продукция:</h1>

<table cellpadding="10" cellspacing="0" border="1">
<% @products.each do |product| %>
    <tr>
        <td>
            <h2><%= product.title %></h2>
            <p><strong>Описание:</strong> <%= product.description %></p>
        </td>

        <td><img src="<%= product.path_to_image %>" alt="<%= product.title %>"></td>

        <td>
            <p><strong>Цена:</strong> <%= product.price %> руб.</p>
            <p><strong>Размер:</strong> <%= product.size %> см</p>
            <p><button onclick="add_to_cart(<%= product.id %>)">Добавить в корзину</button></p>
        </td>
    </tr>
<% end %>
</table>
```
#### Структура данных для нашей корзины заказа в PizzaShop - хеш:

- key - id продукта
- value - количество

Перед написанием на js напишем на ruby:
```ruby
order = {}

loop do
  print 'Введите id продукта: '
  id_product = gets.strip

  print "Сколько штук вы хотите заказать: "
  amount_now = gets.strip.to_i

  amount = order[id_product].to_i
  amount += amount_now

  order[id_product] = amount

  puts order.inspect
end
```
> GitHub - krdprog/order-counter-ruby: Order counter (Ruby)
> https://github.com/krdprog/order-counter-ruby/

На сервер мы будем передавать строку.
```txt
1 = 5, 2 = 12, 3 = 0
```
Можно использовать json.

Теперь напишем это на JS:

```js
// + to public/js/main.js

function add_to_cart(id)
{
  var key = 'product_' + id;

  var x = window.localStorage.getItem(key);
  x = x * 1 + 1;
  window.localStorage.setItem(key, x);
}
```
Смотрим на наличие ошибок в консоли Firefox веб-инспектора.

И, смотрим в хранилище веб-инспектора. Или в консоли веб-инспектора набрать:

```txt
localStorage
```
Очистить:
```txt
localStorage.clear();
```
> Когда нужно делать рефакторинг кода. Ответ: всегда.

Сделаем счётчик товаров в корзине:

- ламерский способ - создать переменную counter, но это не будет учитывать наш localStorage

Нам надо пройтись по хешу и посчитать продукты, которые добавили в корзину:

```ruby
# + to app-order-calc.rb

# calculate total number of items in cart
total = 0

order.each do |key, value|
  total += value
end

puts "Total: #{total}"
```
> javascript - Listing localstorage - Stack Overflow
> https://stackoverflow.com/questions/2841029/listing-localstorage#2841042

> javascript - enumerating localStorage properties - Stack Overflow
> https://stackoverflow.com/questions/27946563/enumerating-localstorage-properties

**Домашнее задание:**
- найти про enumerate localStorage
- найти как пройтись по каждому элементу хеша localStorage
- написать JS функцию подсчёта общего количества заказанных продуктов в корзине.

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md