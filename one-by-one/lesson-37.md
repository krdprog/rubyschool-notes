## Урок 37

#### Задача: посчитать все слова в файле:

Для этого используем хеш

```ruby
f = File.open 'file.txt', 'r'

@hh = {}

def add_to_hash word
  if !word.empty?
    word.downcase!

    cnt = @hh[word].to_i
    cnt += 1

    @hh[word] = cnt
  end
end

f.each_line do |line|
  arr = line.split(/\s|\n|\.|,|:|;/)
  arr.each { |word| add_to_hash(word) }
end

f.close

@hh.each do |k, v|
  puts "#{v} - #{k}"
end
```
#### Продолжаем Rails

```bash
rails new blog
```

```bash
rails s
```
Запуск окружения (development, test, production)
```bash
rails s -e development
```

#### rails generate

rails generate [GEN] [параметры]

```bash
rails g
```
Генераторы:
- controller
- model
- ...

```bash
rails g controller home index
```
home - контроллер
index - экшен

Экшен - это обработчик какого-либо URL

Создадим модель:
```bash
rails g model Article title:string text:text
```
```bash
rake db:migrate
```
База данных находится в каталоге /db по адресу /db/development.sqlite

#### rake routes

Эта команда из каталога конфигурации берёт файл /config/routes.rb и нам его выводит.

```bash
rake routes
```
> https://guides.rubyonrails.org/routing.html

Внесём изменения в файл /config/routes.rb
```ruby
Rails.application.routes.draw do
  get 'home/index'

  resources :articles
end
```
и снова посмотрим через:
```bash
rake routes
```
Выведет:
```txt
Prefix       Verb    URI Pattern                  Controller#Action

home_index   GET     /home/index(.:format)        home#index
articles     GET     /articles(.:format)          articles#index
POST                 /articles(.:format)          articles#create
new_article  GET     /articles/new(.:format)      articles#new
edit_article GET     /articles/:id/edit(.:format) articles#edit
article      GET     /articles/:id(.:format)      articles#show
             PATCH   /articles/:id(.:format)      articles#update
             PUT     /articles/:id(.:format)      articles#update
             DELETE  /articles/:id(.:format)      articles#destroy
```

Открыв http://localhost:3000/articles/new выпадет ошибка Routing Error. uninitialized constant ArticlesController

Т.к. у нас нет пока контроллера articles который отвечает за эту сущность.

Создадим контроллер:
```bash
rails g controller articles
```
> В /app/controllers/ наши контроллеры

Открыв http://localhost:3000/articles/new выпадет ошибка Unknown action The action 'new' could not be found for ArticlesController

Добавим экшен new в наш контроллер /app/controllers/articles_controller.rb

```ruby
class ArticlesController < ApplicationController

  def new
  end

end
```
Открыв http://localhost:3000/articles/new выпадет ошибка ActionController::UnknownFormat in ArticlesController#new. ArticlesController#new is missing a template for this request format and variant.

Т.к. отсутствует шаблон (представление)

Представления расположены в каталоге/app/views/

Создадим файл /app/views/new.html.erb

```bash
touch /app/views/new.html.erb
```
> Всё вышеописанное делается с помощью одной команды, сейчас мы просто разбираемся в работе Rails.

Далее, в /app/views/new.html.erb добавим поля:

> Синтаксис похож на sinatra, но есть и отличия

```ruby
<h1>New article</h1>

<%= form_for :article do |f| %>
test
<% end %>
```
<%= form_for %> - форм билдер. Существуют разные форм билдеры

Создаст форму (см. код страницы):

```html
<form action="/articles/new" accept-charset="UTF-8" method="post"><input name="utf8" type="hidden" value="&#x2713;" /><input type="hidden" name="authenticity_token" value="9hU/+pVEeHGrtOn7Rt9typABgnGxRzLVYnLA6wP49SOVEHSu2CPRjhvPRCHl/q77tNc1FENqAzqVCOASsQdzfg==" />
test
</form>
```
Выведем наши поля формы:

```ruby
<h1>New article</h1>
<%= form_for :article, url: '/articles' do |f| %>
  <p><%= f.label :title %>
  <%= f.text_field :title %></p>

  <p><%= f.label :text %>
  <%= f.text_area :text %></p>

  <p><%= f.submit %></p>
<% end %>
```
Создали форму, но при нажати на кнопку submit ничего не произойдёт, т.к. у new у нас метод GET. (было <%= form_for :article do |f| %>)

Для POST используется экшен create. Нам надо перенаправить форму на этот URL /articles

Добавим к уже существующему коду:
```ruby
<%= form_for :article, url: '/articles' do |f| %>
```
или так:

```ruby
<%= form_for :article, url: articles_path do |f| %>
```
Добавим в app/controllers/articles_controller.rb экшен:

```ruby
  def create
  end
```
Теперь при нажатии на кнопку будет выпадать ошибка: No template found for ArticlesController#create, т.к. отсутствует представление create

Попробуем вывести на экран параметры, которые нам передаются.

```ruby
  def create
    render plain: params[:article].inspect
  end
```
У нас есть create и к нему идут обращения.

render - функция
plain: params[:article].inspect - параметр функции
plain: - ключ хеша
params[:article].inspect - значение хеша

#### Домашнее задание:
- сделать страницу /home/contacts с формой для контактов (обычная как в синатре)
- сделать так, чтобы на сервер передавались email и message из формы контактов

Подсказка:
```bash
rails g controller ...
```
Сделать простым способом без REST.

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |