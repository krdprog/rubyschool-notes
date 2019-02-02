## Урок 31

#### Как работает HTTP

> Изучи ссылку: How exacty HTTP protocol works? - Stack Overflow
> https://stackoverflow.com/questions/20918321/how-exacty-http-protocol-works
> и what happens when you type in a URL in browser - Stack Overflow
> https://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser

Порт - это абстракция операционной системы, или абстракция протокола.

```txt
1. Resolve domain if not an IP (DNS query)
2. Open port 80 by default if not SSL and not overridden by a colon (http: //host:port/)
3. Send request (#1) for http: //host/uri/here?other=stuff&too
4. Receive response (#2)
5. Close
```
Порты:

- 80 - http
- 443 - https

Запрос:

```txt
GET /uri/here?other=stuff&too HTTP/1.1
Host: host
Other: Headers, too.  Such as cookies
Header: Value
```
Ответ:

```txt
HTTP/1.1 200 OK
Other: Headers, too.  Such as cookies
Header: Value

<html>Actual HTTP payload is here, could be HTML data, downloaded file data, etc.
```

> Изучи ссылку:
> Сетевая модель OSI — Википедия
> https://ru.wikipedia.org/wiki/%D0%A1%D0%B5%D1%82%D0%B5%D0%B2%D0%B0%D1%8F_%D0%BC%D0%BE%D0%B4%D0%B5%D0%BB%D1%8C_OSI

#### what happens when you type in a URL in browser

```txt
1. browser checks cache; if requested object is in cache and is fresh, skip to #9
2. browser asks OS for server's IP address
3. OS makes a DNS lookup and replies the IP address to the browser
4. browser opens a TCP connection to server (this step is much more complex with HTTPS)
5. browser sends the HTTP request through TCP connection
6. browser receives HTTP response and may close the TCP connection, or reuse it for another request
7. browser checks if the response is a redirect or a conditional response (3xx result status codes), authorization request (401), error (4xx and 5xx), etc.; these are handled differently from normal responses (2xx)
8. if cacheable, response is stored in cache
9. browser decodes response (e.g. if it's gzipped)
10. browser determines what to do with response (e.g. is it a HTML page, is it an image, is it a sound clip?)
11. browser renders response, or offers a download dialog for unrecognized types
```
> [what happens when you type in a URL in browser - Stack Overflow](https://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser)

#### stateless (без состояния)

HTTP - это протокол без состояния, т.к. сервер не прицепляет никакого специального значения.

> Изучить ссылку: Why is it said that "HTTP is a stateless protocol"? - Stack Overflow
> https://stackoverflow.com/questions/13200152/why-is-it-said-that-http-is-a-stateless-protocol

> Протокол без сохранения состояния — Википедия
> https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%BE%D1%82%D0%BE%D0%BA%D0%BE%D0%BB_%D0%B1%D0%B5%D0%B7_%D1%81%D0%BE%D1%85%D1%80%D0%B0%D0%BD%D0%B5%D0%BD%D0%B8%D1%8F_%D1%81%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D1%8F

Если сервер разрывает соединение, как же сделать так, чтобы работала авторизация и мы были уверены, что это тот самый пользователь. Для этого существует специальный механизм.

> Программа charles-proxy - анализ трафика

> url unescape online (search query)

> Изучи ссылку: RFC 2616 - Hypertext Transfer Protocol -- HTTP/1.1
> https://tools.ietf.org/html/rfc2616

#### gem devise - для авторизации

> [GitHub - plataformatec/devise: Flexible authentication solution for Rails with Warden.](https://github.com/plataformatec/devise)

> [Аутентификация в Rails-приложениях с помощью Devise. Часть 1: базовая настройка / Хабр](https://habr.com/post/208056/)

#### Vanilla.js == JS

> Vanilla JS - http://vanilla-js.com/

### Функция в js
```js
function foo()
{
  alert('Hello!');
}
```
В JS функция вызывается по хэндлеру/событию (обработчик):

```html
<button onclick="foo()">Click me!</button>

<script>
  function foo() {
    alert("Hello!");
  }
</script>
```

Переменные:
```html
<button onclick="foo()">Click me!</button>

<script>
  var x = 1;

  function foo() {
    alert(x);
    x = x+1;
  }
</script>
```

---
**Следующий урок:**  https://github.com/krdprog/rubyschool-notes/blob/master/one-by-one/lesson-32.md