
# Комманды

- Версия git
> git --version

- Установки
> git config --global user.name "Yuriy Teplyakov"
> git config --global user.email "...@mail.ru"

- Правильность настройки
> git config --list

- Местоположение конфигурационного файла
> git config --list --show -origin

- Создание текущей папки как git проект
> git init путь

- Индексация всех новых файлов
> git add .

- Только одного выбранного файла
> git add hello.txt

- Снимок состояния (фиксация отслеживаемых файлов)
> git commit -m 'комментарий'
> где -m параметр для комментария к коммиту

- блокнот по умолчанию как редактор
> git config --global core.editor notepad

- Узнать какие изменения внесены в кодовую базу проекта
> git diff

- изменения которые сейчас находятся в индексе
> git diff --cached

- совмещенная команда
> git commit -am 'комментарий'

- Удаление файла: откат изменений к предыдущему коммиту
> git checkout --hello.txt

- Перевод файла в НЕиндексируемое состояние
> git reset HEAD hello.txt

- История изменений от более свежих к более ранним
> git log
> пробел - двигаться дальше
> q -выход

- ограничение по количеству выводимых коммитов
> git log -2

- детальные изменения для каждого из коммитов
> git log -2 -p

- статистика для каждого из коммитов (список измененных файлов)
> git log -2 --stat

- Упрощение в одну строку
> git log --pretty=oneline

- Коммиты за определенный интервал времени
> git log --since = 2.week
> git log --until = 1.week
>  git log --author = "имя"

## Ветки

- Создание новой ветки
> git branch feature

- Переключение на новую ветку
> git checkout feature

- Одновременное создание и переключение на новую ветку
> git checkout -b test

- Переименование ветки
> git branch -m feature to_delete

- Удаление ветки
> git branch -D to_delete

### Слияние веток

"В момент слияния нужно находится в ветке куда будем сливать изменения"

- Свержение из ветки test в ветку main
> git merge test

## Удаленные репозитории

- Отправка изменений на удаленный сервер
> git push -u origin main

## Версионирование

> git tag v1.3.0
> git tag -a V1.4.0 -m "новый тэг"

- Поиск по шаблону
> git tag -l v1.4*

- Удаление тэга
> git tag -d v1.3

- Ссылка на последний коммит
> git show HEAD

## Связь между локальным и удаленным репозиториями

> git remote add origin

- Убедиться в том что соединение прошло успешно
> git remote -v

- (хэш коммита) сброс текущего состояния истории
> git reset

- История изменений коммитов и всех операций совершенных во время работы с репозиторием
> git reflog

- Не сбрасывает состояние репозитория, а создает новый коммит, который отменяет действия совершенные в прошлом
> git revert

``` py
git remote add origin https://github.com/yuriyteplyakov/mywiki.git

git push origin master --force (принудительно)

для pages на github
git pull origin gh-pages --rebase (Принудительное обновление БД для Actions на gh.com) 

mkdocs gh-deploy --force

# без верхней команды не происходит git push
# возможно проблемы с правами доступа

git pull origin master  (изменения с удаленного на локальный репозиторий)
git push origin master  (изменения с локального на удаленный репозиторий)

git add . (записать изменение всех файлов)

git commit -m 'TMC 6510 start' (коммит)
```