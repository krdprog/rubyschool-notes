## Урок 45

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

## Навигация по конспектам (по-одному уроку в файле):

|  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |  N  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|  [01](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-01.md)  |  [02](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-02.md)   |  [03](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-03.md)   |  [04](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-04.md)  |  [05](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-05.md)  |  [06](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-06.md)  |  [07](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-07.md)  |  [08](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-08.md)  |  [09](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-09.md)  |  [10](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-10.md) |
|  [11](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-11.md) |  [12](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-12.md) |  [13](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-13.md) |  [14](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-14.md) |  [15](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-15.md) |  [16](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-16.md) |  [17](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-17.md) |  [18](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-18.md) |  [19](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md) |  [20](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-20.md) |
|  [21](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-21.md) |  [22](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-22.md) |  [23](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-23.md) |  [24](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-24.md) |  [25](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-25.md) |  [26](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-26.md) |  [27](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-27.md) |  [28](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-28.md) |  [29](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md) |  [30](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-30.md) |
|  [31](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-31.md) |  [32](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md) |  [33](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-33.md) |  [34](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-34.md) |  [35](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-35.md) |  [36](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-36.md) |  [37](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-37.md) |  [38](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-38.md) |  [39](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-39.md) |  [40](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-40.md) |
|  [41](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-41.md) |  [42](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-42.md) |  [43](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-43.md) |  [44](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-44.md) |  [45](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-45.md) |  [46](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md) |  [47](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-47.md) |  [48](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-48.md) |  [49](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-49.md) |  [50](https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-50.md) |