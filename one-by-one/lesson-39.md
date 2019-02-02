## Урок 39

resource и resources отвечают за REST маршруты приложения, они записываются в /config/routes.rb

**resource (profile):**

- показать - show
- создать - new (отобразить форму. GET), create (отправить форму. POST)
- редактировать - edit, update
- удалить - destroy

**resources (articles):**

- показать список - index
- показать - show
- создать - new (отобразить форму. GET), create (отправить форму. POST)
- редактировать - edit, update
- удалить - destroy

#### Продолжим делать блог

У нас есть http://localhost:3000/articles/new и http://localhost:3000/contacts

Нам нужно создать новую статью. Сделаем это. Откроем /app/controllers/articles_controller.rb и доделаем метод create

Посмотрим какие поля существуют в нашей сущности Article
```bash
rails console
Article.attribute_names
```
=> ["id", "title", "text", "created_at", "updated_at"]

Внесём изменения в /app/controllers/articles_controller.rb:

```ruby
class ArticlesController < ApplicationController

  def new
  end

  def create
    @article = Article.new(article_params)
    if @article.valid?
      @article.save
      redirect_to @article
    else
      render action: 'new'
    end
  end

  private

  def article_params
    params.require(:article).permit(:title, :text)
  end

end
```
Теперь нам надо создать представление для create. Создадим: /app/views/articles/create.html.erb

```html
<h2>Спасибо!</h2>
<p>Статья создана</p>
```

Посмотреть список статей в консоли rails:
```bash
rails console
Article.all
```

Добавим ссылку на все статьи в /app/views/articles/create.html.erb

Так не делают:
```html
<a href="/articles">Показать все статьи</a>
```
Делают так:
```ruby
<%= link_to "Показать все статьи", articles_path %>
```

> Для борьбы с двойным сабмиттом существует паттерн PRG (Post Redirect Get)

Для этого мы добавили строку:
```ruby
redirect_to @article
```
У нас происходит редирект на show поэтому представление create нам теперь не нужно, его можно удалить.

Добавим в app/controllers/articles_controller.rb:
```ruby
  def show
    @article = Article.find(params[:id])
  end
```

Создадим представление /app/views/articles/show.html.erb

```ruby
<h1><%= @article.title %></h1>

<p><%= @article.text %></p>
```

#### Вывод списка статей

Список статей будет доступен по адресу http://localhost:3000/articles

Добавим в контроллер /app/controllers/articles_controller.rb экшен index:

```ruby
def index
  @articles = Article.all
end
```

И, создадим вьюху /app/views/articles/index.html.erb

```ruby
<% @articles.each do |article| %>
  <h3><%= article.title %></h3>
  <p><%= article.text %></p>
  <p><%= link_to  "Show article", article_path(article) %> | <%= link_to  "Edit article", edit_article_path(article) %></p>
  <hr>
<% end %>
```

#### Редактирование статьи (edit, update):

Кнопка на редактирование:

```ruby
<%= link_to "Edit article", edit_article_path(article) %>
```

Добавим в /app/controllers/articles_controller.rb:
```ruby
def edit
  @article = Article.find(params[:id])
end
```
И, создадим вьюху /app/views/articles/edit.html.erb:
```ruby
<h1>Edit article</h1>

<%= form_for :article, url: article_path(@article), method: :patch do |f| %>
<p>
  <%= f.label :title %>
  <%= f.text_field :title %>
</p>
<p>
  <%= f.label :text %>
  <%= f.text_area :text %>
</p>

<p>
  <%= f.submit %>
</p>

<% end %>
```

Добавим в контроллер /app/controllers/articles_controller.rb экшен update:

```ruby
def update
  @article = Article.find(params[:id])

  if @article.update(article_params)
    redirect_to @article
  else
    render action: 'edit'
  end
end
```

#### Контроллер и роутинг статических страниц

```bash
rails g controller Pages
```
Создаётся контроллер /app/controllers/pages_controller.rb, внесём код:
```ruby
class PagesController < ApplicationController

  def terms
  end

  def about
  end

end
```

И, пропишем маршруты в /config/routes.rb:

```ruby
  get 'terms' => 'pages#terms'
  get 'about' => 'pages#about'
```

И, создадим вьюхи /app/views/pages/terms.html.erb и /app/views/pages/about.html.erb

> Изучи ссылку: Rusrails: Командная строка Rails
> http://rusrails.ru/a-guide-to-the-rails-command-line

#### Домашнее задание:
- переписать из rake routes
- сделать destroy (delete)

> Ссылка на репозиторий с учебным блогом на Rails:
> https://github.com/krdprog/RailsBlog-rubyschool

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |