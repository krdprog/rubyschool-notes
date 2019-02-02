## Урок 35

#### Разбор вопросов на интервью

> [15 Questions to Ask During a Ruby Interview · GitHub](https://gist.github.com/krdprog/64a463de21fe77a8946019fde6662d67#what-is-unit-testing-in-classical-terms--what-is-the-primary-technique-when-writing-a-test)

- Что такое юнит-тесты. Юнит-тесты предназначены для тестирования отдельных модулей программы (к частям, которые не делимы).

```ruby
require "test/unit"

class Brokened
  def uh_oh
    "I needs fixing"
  end
end

class BrokenedTest < Test::Unit::TestCase
  def test_uh_oh
    actual = Brokened.new
    assert_equal("I'm all better!", actual.uh_oh)
  end
end
 #=> Started
 #=> F
 #=> Finished in 0.663831 seconds.
 #=>
 #=>   1) Failure:
 #=> test_uh_oh:11
 #=> <"I'm all better!"> expected but was
 #=> <"I needs fixing">.
 #=>
 #=> 1 tests, 1 assertions, 1 failures, 0 errors
```
> Вызываем метод и проверяем, что результат совпадает с эталоном

test coverage - покрытие тестами

Интеграционное тестирование - тестирование сайта в браузере

Как много надо покрывать кода тестами?

Юнит тестирование нужно, чтобы при возрастании сложности приложения функциональность большого приложения сохранялась.

- Статическая и динамическая типизация

```java
// Java
public boolean isEmpty(String s) {
  return s.length() == 0;
}
```
```ruby
# ruby
def empty?(s)
  return s.size == 0
end
```
В руби меньше кода и он более гибкий.

> [oDesk and Elance Tests 2015](https://web.archive.org/web/20150220030931/http://odesk-tests.com:80/)

> [Ruby on Rails Test 2015 - oDesk](https://web.archive.org/web/20150206003956/http://odesk-tests.com:80/tests/307/questions)

#### Продолжаем PizzaShop

Нам надо разбить строку результата заказа в Ruby
```txt
"product_1=9,product_3=2,"
```
Создадим отдельную руби-программу:
```ruby
# pizza-shop-other.rb

orders = "product_1=9,product_3=2,"

s1 = orders.split(/,/)

s1.each do |x|
  s2 = x.split(/=/)
  s3 = s2[0].split(/_/)

  key = s3[1]
  value = s2[1]

  puts "Priduct Id: #{key}, number of items: #{value}"
end
```
Это не эффективный способ, лучше использовать json (позволяет поддерживать формат данных любой сложности).

Перепишем в метод:
```ruby
# + to app.rb

post '/cart' do
  orders_input = params[:orders]

  @items = parse_orders_input orders_input

  @items.each do |item|
    # id, cnt
    item[0] = Product.find(item[0])
  end

  erb :cart
end

# Parse orders line:
def parse_orders_input orders_input

  s1 = orders_input.split(/,/)

  arr = []

  s1.each do |x|
    s2 = x.split(/=/)
    s3 = s2[0].split(/_/)

    id = s3[1]
    cnt = s2[1]

    arr2 = [id, cnt]
    arr.push arr2
  end

  return arr
end
```
```ruby
# + to /views/cart.erb

<h2>Заказанные товары:</h2>

<table border="1" cellspacing="0" cellpadding="10">
  <tr>
    <th>Товар</th>
    <th>Цена</th>
    <th>Количество</th>
  </tr>

  <% total_qty = 0 %>
  <% total_price = 0 %>

  <% @items.each do |item| %>
  <tr>
    <td><%= item[0].title %></td>
    <td><%= item[0].price %> руб.</td>
    <td><%= item[1] %></td>
  </tr>

  <% total_qty += item[1].to_i %>
  <% total_price += item[0].price * total_qty %>
  <% end %>

  <tr style="background: yellow;">
    <td><strong>Сумма:</strong></td>
    <td><%= total_price %> руб.</td>
    <td><%= total_qty %></td>
  </tr>

</table>
```
#### Отношение между сущностями:
- один ко многим
- многие к одному
- многие ко многим
- один к одному

Доделаем PizzaShop не лучшим, но простым путём. Создадим форму и отправим заказ на сервер (сохраним в базе данных).

> GitHub - krdprog/PizzaShop-rubyschool: Pizza Shop (rubyschool project). Ruby, Sinatra, ActiveRecord, JS, localStorage
> https://github.com/krdprog/PizzaShop-rubyschool

#### Домашнее задание:
- сделать модель Order с полями из формы cart.erb, не забыть про миграцию
- добавить post-обработчик /place_order в котором получать данные из страницы и сохранять в базу данных
- выводить на экран сообщение "Заказ принят"
- выводить на экран страницу со всеми принятыми заказами

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md