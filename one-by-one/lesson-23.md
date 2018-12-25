### Урок 23

```ruby
<script src="script.js"></script>
```

jQuery:
```ruby
$('#aaa').css('background-color','yellow');
$('.ccc').css('background-color','red');
```

Код в { ... } выполнится после того, как загрузится вся страница:
```ruby
<body>
  <script>
    $(function() {
        ...
      });
  </script>
</body>
```