## Урок 42

Продолжим рассматривать Rspec и тестирование.

Добавим к нашим тестам в файле hero_spec.rb

```ruby
  it "can power down" do
    hero = Hero.new 'foo'

    expect(hero.power_down).to eq 90
  end

  it "displays full hero info" do
    hero = Hero.new 'foo'

    expect(hero.hero_info).to eq "Foo has 100 health"
  end
```

У нас часто повторяется код hero = Hero.new 'foo', и это не совпадает с DRY (Don`t Repeat Yourself), ниже мы его оптимизируем, добавив before-do-end

Рефакторинг hero_spec.rb:

```ruby
require './hero'

describe Hero do

  before do
    @hero = Hero.new 'foo'
  end

  it "has a capitalized name" do
    expect(@hero.name).to eq 'Foo'
  end

  it "can power up" do
    expect(@hero.power_up).to eq 110
  end

  it "can power down" do
    expect(@hero.power_down).to eq 90
  end

  it "displays full hero info" do
    expect(@hero.hero_info).to eq "Foo has 100 health"
  end

end

```

**Тесты должны быть:**

- надёжные (reliable) - дают тот же результат - без зависимостей от соединения, от БД и т.п.
- easy to write - если тест пишется не легко, то что-то не так с тем, что тестируем.
- easy to understand - лёгкие для понимания другими программистами.
- скорость не особо важна
- не важно DRY

#### Rspec Matchers

> Изучить ссылку: https://relishapp.com/rspec/rspec-expectations/docs/built-in-matchers

**Сделаем ещё одно приложение и тест:**

car.rb:

```ruby
class Car

  Miles_Per_Gallon = 20

  attr_reader :fuel

  def initialize
    @fuel = 0
  end

  def add_fuel amount
    @fuel += amount
  end

  # Как далеко мы сможем проехать:
  def range
    @fuel * Miles_Per_Gallon
  end

end

# car = Car.new
# car.add_fuel 10
# puts "Range is #{car.range}"
```

car_spec.rb:

```ruby
require "./car"

describe Car do
  it "must return range" do
    # arrange
    car = Car.new
    # act
    car.add_fuel 10
    # assert
    expect(car.range).to eq 200
  end
end
```

**Структура тестов:**

```text
# arrange
# act
# assert
```

```ruby
# arrange - подготовка объекта для проведения теста
  car = Car.new
# act - действие
  result = car.add_fuel 10
# assert - проверка действия
  expect(result) ...
```

> Заметка по Issue на Github. У issue есть номер (например #12), если при коммите указать этот номер, то issue сама закроектся и пометится как выполненная.

### Devise

Devise - гем для авторизации

Добавим в Gemfile:

```ruby
gem 'devise'
```

```bash
bundle
```
**Вывести в терминал список генераторов в системе:**

```bash
rails g
```

```text
Devise:
  devise
  devise:controllers
  devise:install
  devise:views
```

Введём:

```bash
rails g devise:install
```

План, как подключать devise:

1. gem 'devise' в Gemfile
2. rails g devise:install

Проверить есть ли строка в файле config/environments/development.rb:

```ruby
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

Проверить, что в  /config/routes.rb указано:

```ruby
root to: "home#index"
```

Добавить в app/views/layouts/application.html.erb для флеш-сообщений:

```html
<p class="notice"><%= notice %></p>
<p class="alert"><%= alert %></p>
```

Для кастомизации форм:

```bash
rails g devise:views
```

**Далее, создадим модель пользователя:**

```bash
rails g devise User
```

Devise создал параметры: e-mail, зашифрованный пароль, токен для сброса пароля.

**Наберём:**

```bash
rake db:migrate
```

И, запустим:

```bash
rails s
```

### Задача: чтобы мы могли просматривать статьи, но не могли их создавать.

- http://localhost:3000/articles
- http://localhost:3000/articles/new

Откроем /app/controllers/articles_controller.rb и добавим:

> Примечание: начиная с Rails 5 синтаксис before_filter устарел и заменён на before_action

```ruby
before_action :authenticate_user!
```

Добавим в /app/views/layouts/application.html.erb:

```html
<p><a href="/users/sign_in">Sign In</a> | <a href="/users/sign_out" data-method="delete">Sign Out</a></p>
```

Далее, мы заменим эти ссылки на ссылки с именноваными маршрутами.

> Документация по гему devise - https://github.com/plataformatec/devise

> Статья на Хабре по devise - https://habr.com/ru/post/208056/

> Посмотреть примеры - https://github.com/plataformatec/devise/wiki/Example-applications


---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |