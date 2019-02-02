## Урок 18

#### Задача с password:
```ruby
# прочитать строки из файла password.txt
input = File.open("password.txt", "r")
output = File.open("password_2.txt", "w")

# вывести на экран пароли длиной 6 символов
# записать это в отдельный файл

while (line = input.gets)
  line.strip!
  if line.size == 6
    puts line
    output.write "#{line}\n"
  end
end

input.close
output.close
```
```ruby
# вывести на экран пароли длиной 6 символов
# записать это в отдельный файл

input = File.open("password_2.txt", "r")

# Enter your password
print "Enter your password: "
password = gets.strip

# Your password is weak / not weak
while (line = input.gets)
  line.strip!

  if line == password
    puts "Your password is weak!"
    exit
  end
end

puts "Your password is hard!"

input.close
```

GET - получить (браузер)
POST - отправить (браузер)

> Изучи ссылку: RFC HTTP
>  http://lib.ru/WEBMASTER/rfc2068/

#### Использование библиотеки Net::HTTP

> Изучи ссылку: Class: Net::HTTP (Ruby 2.4.1)
https://ruby-doc.org/stdlib-2.4.1/libdoc/net/http/rdoc/Net/HTTP.html


Загрузить страницу:
```ruby
require 'net/http'

page = Net::HTTP.get('krdprog.ru', '/index.html')
puts page
```
Вариант 2:
```ruby
require 'net/http'
require 'uri'

uri = URI.parse "http://krdprog.ru/index.html"
response = Net::HTTP.get uri

puts response
```

#### Метод post_form

```ruby
require 'net/http'
require 'uri'

Net::HTTP.post_form URI('http://www.example.com/search.cgi'),
{ "q" => "ruby", "max" => "50" }
```
> Class: Net::HTTP (Ruby 2.4.1) post_form
https://ruby-doc.org/stdlib-2.4.1/libdoc/net/http/rdoc/Net/HTTP.html#method-c-post_form

```ruby
require 'net/http'
require 'uri'

uri = URI.parse "https://duckduckgo.com/index.html"
response = Net::HTTP.post_form uri, :x => "ruby"

puts response
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-19.md