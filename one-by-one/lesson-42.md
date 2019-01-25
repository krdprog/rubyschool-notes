### Урок 42

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
<p><a href="/users/sign_in">Sign In</a> | <a href="/users/sign_up">Sign Up</a></p>
```

Чтобы сделать Sign Out, надо добавить:

```html
<a href="/users/sign_out" data-method="delete">Sign Out</a>
```

> Документация по гему devise - https://github.com/plataformatec/devise

> Статья на Хабре по devise - https://habr.com/ru/post/208056/

> Посмотреть примеры - https://github.com/plataformatec/devise/wiki/Example-applications


---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md