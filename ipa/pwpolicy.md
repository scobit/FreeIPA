#### Поиск политики паролей для группы
```
ipa pwpolicy-find --group=GroupName
```

#### Показать глобальную политику паролей
```
ipa pwpolicy-show
```
#### Добавить политику паролей
```
ipa pwpolicy-add GroupNameToWhichAddPolicy --minlife=1 --maxlife=7700 --history= --priority=1

where:
--minlife=INT       Минимальный срок действия пароля (в часах)
--maxlife=INT       Максимальное срок действия пароля (в днях)
--history=INT       Хранение количества историй изменения пароля
--priority=INT      Приоритет политики, чем выше цифра, тем ниже приоритет

```

