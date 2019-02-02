## Урок 41

**Когда используется единственное число, а когда множественное?**

- таблицы в БД - множественное число
- модели - единственное число
- контроллеры - множественное число

> В книге Rails. Гибкая разработка веб-приложений (Руби, Томас, Хэнссон) прочитать про соглашения об именах.

#### Типы связей

> http://rusrails.ru/active-record-associations

**Тип 1 - * (one-to-many)**

```text
Article            |  Comment

has_many :comments |  belongs_to :article
id                 |  id, article_id
```

**Тип 1 - 1 (one-to-one)** - помогает нормализовать БД

```text
Order              |  Address

has_one :address   |  belongs_to :order
id                 |  id, order_id
```

Нормализация, денормализация. Плюсы и минусы подходов. В варианте выше потребуется ещё 1 запрос к базе данных.

> Понятие complex_type - сложный тип (изучить)

**Тип * - * (many-to-many)**

```text
Tag                               |  Article

таблица tags                      |  таблица articles
id                                |  id
has_and_belongs_to_many :articles | has_and_belongs_to_many :tags
```

для связи между ними создаётся ещё одна таблица tags_articles (tag_id, article_id)

> Изучить: http://www.rusrails.ru/active-record-associations#foreign_key

#### Вывод комментариев в представлении статьи:

Добавим в /app/views/articles/show.html.erb:

```ruby
<h3>Комментарии:</h3>

<%= 'Без комментариев' if @article.comments.empty? %>

<% @article.comments.each do |comment| %>

<p><strong>Автор:</strong> <%= comment.author %></p>
<p><%= comment.body %></p>
<hr>

<% end %>
```

#### Ещё раз про ActiveRecord

**Вспомним CRUD:**

- Create - (new) - .create - .save
- Read - .where - .find(3), .all
- Update - (update)
- Delete - (destroy)

> https://github.com/rails/strong_parameters

> https://guides.rubyonrails.org/action_controller_overview.html#more-examples

**Вспомним REST:**

- resource
- resources

Можно вкладывать, получаются длинные URL:

```ruby
resources :article do
  resources :comments
end
```

```ruby
x = 2 != 3
puts x #=> true
```

### Rspec - фреймворк для тестирования приложений

```bash
gem install rspec
```

Запуск тестов:

```bash
rspec
```

**В Rspec существует 2 слова:**

- describe
- it

**Создадим и протестируем героя компьютерной игры.**

> https://github.com/krdprog/rspec-demo-rubyschool - репозиторий Rspec-demo

Создадим файл hero.rb

```ruby
class Hero

  def initialize(name, health=100)
    @name = name.capitalize
    @health = health
  end

  attr_reader :name

  def power_up
    @health += 10
  end

  def power_down
    @health -= 10
  end

  def hero_info
    "#{@name} has #{@health} health"
  end

end
```

Создадим файл hero_spec.rb:

```ruby
require './hero'

describe Hero do

  it "has a capitalized name" do
    hero = Hero.new 'foo'

    expect(hero.name).to eq 'Foo'
  end

  it "can power up" do
    hero = Hero.new 'foo'

    expect(hero.power_up).to eq 110
  end

end
```

И, запустим тест:

```bash
rspec hero_spec.rb
```

Написание тестов в большом приложении - это вклад в будущее, защита приложения от ошибок.

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |