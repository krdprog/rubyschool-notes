## Конспект RubySchool.us [3]

### Урок 20

app.rb
```ruby
require "sinatra"

get '/' do
	"Hello Foo Bar"
end

get '/contacts' do
	@title = "Contacts"
	@message = "Hello all! Phone me now."
	erb :message
end

get '/faq' do
	@title = "FAQ about our products"
	@message = "It`s FAQ"
	erb :message
end
```
views/message.erb
```ruby
<h1><%= @title %></h1>

<p><%= @message %></p>
```

12 мин