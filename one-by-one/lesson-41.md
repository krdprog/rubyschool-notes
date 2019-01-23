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

**Тип 1 - 1 (one-to-one)** - помогает нормализовать БД

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

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md