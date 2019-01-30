### Урок 45

Принцип работы метода params в контроллерах: в params хранятся все параметры которые передаются из браузера в приложение.

```ruby
private

def article_params
  params.require(:article).permit(:title, :text)
end
```

> https://stackoverflow.com/questions/18424671/what-is-params-requireperson-permitname-age-doing-in-rails-4

> https://api.rubyonrails.org/classes/ActionController/Parameters.html#method-i-require

> https://api.rubyonrails.org/classes/ActionController/Parameters.html#method-i-permit

> https://api.rubyonrails.org/classes/ActionController/Parameters.html

```text
Browser ===> Server ===> Controller ===> ActiveRecord ===> Database
                                             ||
                                             ===> Error
```

**Правило:** никогда не верьте тому, что передаёт клиент.

#### Are redirect_to and render exchangeable?

> https://stackoverflow.com/questions/7493767/are-redirect-to-and-render-exchangeable

Если используется render когда юзер обновляет страницу, то он засабмитит предыдущий POST-запрос, это может отправить дополнительные данные.

Если используется redirect_to, то это будет выгдядеть как редирект.

**Post/Redirect/Get (PRG) pattern:**

> https://en.wikipedia.org/wiki/Post/Redirect/Get

#### Хелперы:

Все хелперы расположены в каталоге /app/helpers/

Для каждого контроллера существует хелпер, можно вызывать различные методы из этих хелперов, из разных представлений. Хелпер - работает между контроллером и представлением. Чтобы не вставлять код в представление.

Чтобы не дублировать код в каждом представлении.

Логику в представлениях писать неправильно, надо выносить в хелперы. Представления предназначены для того, чтобы отображать данные. Нехорошо размазывать логику по всему приложению.

Один хелпер создаётся для одного контроллера, но все хелперы доступны всем контроллерам.

Существуют также **встроенные хелперы** в Rails, например **debug**, выведет список параметров.

```ruby
<%= debug(params) %>
```

Ещё посмотреть:

```ruby
<%= simple_format(@foo) %>
```

И, см. автоматическая подсветка ссылок - **autolinks**

А, также **truncate - если есть длинная строка, то обрезается под указанный размер:**

```ruby
<%= truncate(@foo, length: 20) %>
```

И, уже известный нам хелпер link_to.

> См. ещё: http://rusrails.ru/action-view-overview

> https://guides.rubyonrails.org/form_helpers.html

#### Как устроен процесс разработки в компании (CI, CD)

- CI - continuous integration - непрерывная интеграция - автотест при коммитах в общий репозиторий
- CD - continuous delivery - непрерывная доставка - автозаливка на веб-сервер

```ruby
programmer  owner
   |   ______|
hipchat ______ github -------
   |__________ test server__|
                 |
                www (staging) --- QA
```
Integration tests - watir, selenium

> https://github.com/watir/watir

> https://github.com/SeleniumHQ/selenium

> KANBAN-доска: http://kanbanflow.com/

> http://trello.com/

#### Vargant

> https://www.vagrantup.com/

> https://github.com/rails/rails-dev-box

**Домашнее задание:**

- поставить Vargant
- привязать сущность article в блоге к пользователю. Сделать так, чтобы другие пользователи не могли редактировать статьи.

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md