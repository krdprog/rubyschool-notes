### Урок 40

#### Повторение:

**Чтобы показать обычные статические страницы (типи /terms, /about) нам надо:**

- создать контроллер (rails g controller ...)
- добавить actions (методы terms, about)
- добавить views (terms.html.erb, about.html.erb)
- добавить в routes.rb маршруты (get 'terms' => 'pages#terms')

В синатре это занимало 2 действия:

- маршрут
- views

**Создание сущности:**

- создание модели (rails g model ...)
- rake db:migrate
- добавить маршрут в routes.rb - resource или resources
- добавить контроллер
- в контроллер добавить actions (index, show, new, edit, create, update, destroy)
- добавить views

### Продолжим делать RailsBlog, доделаем удаление Article:

Чтобы удалить сущность, надо сделать 2 действия:

- найти сущность по id
- удалить

Добавим в articles_controller.rb метод destroy:

```ruby
def destroy
  @article = Article.find(params[:id])
  @article.destroy

  redirect_to articles_path
end
```

Добавим в файл /views/articles/index.html.erb кнопку удаления статьи:

```html
<%= link_to "Destroy", article_path(article), method: :delete %>
```

или с подтверждением:

```html
<%= link_to "Destroy", article_path(article), method: :delete, data: { confirm: 'Действительно удалить?'} %>
```

К тегу ссылки на удаление добавится атрибут data-method="delete"

Файл turbolinks обрабатывает атрибуты data-* встречающиеся в коде html

**Разбор data-method="delete"**

- скрипт сканирует страницу
- ищет атрибут data-method="delete"
- создаёт форму, которая будет отправлять с помощью метода delete на URL /articles/2 через POST
- устанавливает свой обработчик, при котором при нажатии на ссылку вызывается сабмит формы





---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md