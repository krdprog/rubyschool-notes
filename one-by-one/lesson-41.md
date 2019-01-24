### Урок 41

**Когда используется единственное число, а когда множественное?**

- таблицы в БД - множественное число
- модели - единственное число
- контроллеры - множественное число

> В книге Rails. Гибкая разработка веб-приложений (Руби, Томас, Хэнссон) прочитать про соглашения об именах.

#### Типы связей

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