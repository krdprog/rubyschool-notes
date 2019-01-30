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

Существуют встроенные хелперы в Rails.




---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-46.md