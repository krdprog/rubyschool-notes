## Урок 28

> Мой вариант Sinatra Blog:
> https://github.com/krdprog/sinatra-blog


#### Перенаправление:
```ruby
post '/new'
  # some code

  redirect to '/'
end
```

```ruby
  redirect to ('/post/' + @post_id)
```

#### id поста:
app.rb
```ruby
get '/post/:post_id' do
	@post_id = params[:post_id]
	erb :post
end
```
views/post.erb
```ruby
This is post: <%= @post_id %>
```

#### Домашнее задание:
1. Авторизация для автора
2. Валидация поля комментария

#### Ссылка на документацию Sinatra:
- [sinatra/README.ru.md at master · sinatra/sinatra · GitHub](https://github.com/sinatra/sinatra/blob/master/README.ru.md#%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B7%D0%B0%D1%89%D0%B8%D1%82%D1%8B-%D0%BE%D1%82-%D0%B0%D1%82%D0%B0%D0%BA)

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-29.md