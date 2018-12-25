## Урок 7

Выход из программы:
```ruby
a = true
while a == true
	print "Выйти из программы? (Y/N): "
	answer = gets.strip.capitalize

	if answer == "Y"
		a = false
	end

end
```
```
== - равно
!= - не равно
<>
>=
<=
```

Прерывание программы:
```ruby
exit
```

if И, ИЛИ:
```ruby
# И
if a > 1 && b < 3
  puts "Foo!"
end

# ИЛИ
if a > 1 || b < 3
  puts "Foo!"
end
```