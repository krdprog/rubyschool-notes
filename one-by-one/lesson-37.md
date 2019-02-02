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