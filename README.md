<h1 align="center">Git шпаргалка</h1>
 
 # Команды

``` bash
git --version
git status                 # показать состояние репозитория (отслеживаемые, изменённые, новые файлы и пр.)
git branch                 # работа с ветками (пример: git branch -v покажет ветку и комит на который она указывает)
git add                    # начать отслеживать (добавить под версионный контроль/добавить в индекс) новый файл (пример: git add README) 
git commit                 # сделать коммит (пример: git commit -m "Первый коммит")
git checkout               # переключение между ветками, извлечение отдельных файлов из истории коммитов
git merge                  # вливание веток (пример: git merge имя_ветки_которую_вливаем)
git push                   # отправить в удалённый репозиторий (пример: git push имя_удалённого_репозитория master)
```

# Примеры команд с параметрами:

### Сделать коммит
``` bash
git commit -m "Первый коммит"     # сделать коммит с текстом "Первый коммит"
git commit -a -m "Первый коммит"  # проиндексировать отслеживаемые файлы (ТОЛЬКО отслеживаемые, но НЕ новые файлы) и закоммитить, с текстом "Первый коммит"
```

### Теги
``` bash
git tag v1.0.0             # создать тег на коммите, на который указывает HEAD
git tag -a v1.0.0 9fceb02  # создать тег на определенном коммите
git tag -d v1.0.0          # удалить тег с указанным именем
git tag -n                 # показать все теги, и по 1й строке текста их коммитов
git push origin v1.0.0     # отправить тег на удалённый сервер (или если "v1.0.0"  заменить на "--tags" то отправить все теги).
```

#### Отмена изменений
``` bash
git reset --hard HEAD               # отменяет все изменения до крайнего комита (незакомиченные изменения удалены из индекса и из рабочей директории)
git reset --hard HEAD~              # отменяет все изменения до предыдущего комита  (просто удаляет последний комит, удаляет все из индекса и из рабочей директории)
git reset --hard commit_id          # commit_id - идентификатор коммита, к которому нужно вернуться.
git reset --soft HEAD~              # передвинуть HEAD (и ветку) на предыдущий коммит, но в рабочей директории и индексе оставить все изменения
git reset --soft HEAD~2             # то же, но передвинуть HEAD (и ветку) на 2 коммита назад
git checkout text.txt               # отменить изменения в файле, вернуть состояние файла, имеющееся в индексе
git clean -df                       # удалить неотслеживаемые файлы и директории
```

#### Создание веток
``` bash
git branch new_branch               # создать новую ветку с указанным именем на текущем коммите
git branch new_branch 1234567890    # создать новую ветку с указанным именем на указанном коммите
git checkout -b new_branch          # создать новую ветку с указанным именем и перейти в неё
```
#### Удаление веток (локально)
``` bash
git branch -d new_branch      # сначала проверяет, есть ли у ветки new_branch коммиты, которых нет в главной. Если есть, то удаление отрицается. 
git branch -D new_branch      # принудительное удаление ветки new_branch
```
### Временное сохранение изменений без коммита
``` bash
git stash         # временно сохранить незакоммиченные изменения и убрать их из рабочей директории
git stash pop     # вернуть сохраненные командой git stash изменения в рабочую директорию
```



# Use case: 

#### Изменение последнего коммита
``` bash
# добавить изменения обычным образом в файлах проекта
git add .               # добавляем все в индекс
git commit --amend      # закоммитить изменения ( amend откатит коммит через reset и выполнит новый коммит с новыми данными)
# если изменяемый коммит уже был отправлен, то
git push --force        # принудительно перезаписываем в удаленном репозитории
```

#### Исправляем коммит посередине ветки: autosquash
``` bash
# !!! если репозиторий не публичный, и нет форков !!!
# добавить изменения обычным образом в файлах проекта (внести изменения, исправляющие ошибку)
git add .                             # добавляем все в индекс
git commit --fixup 2b504bbbbbbb       # сделать коммит этих изменений в репозиторий с флагом --fixup, указав хэш «плохого» коммита:
git rebase -i 0023aaaaa --autosquash  # запуск interactive rebase. В качестве отправной точки выбираем коммит, предшествующий «плохому»(родительский коммит)
# окно 1. если нужно изменить описание - помечаем нужный коммит reword (перефразировать описание коммита). Сохранить, закрыть.
# окно 2. редактируем опимание коммита. Сохранить, закрыть.
```


#### Слияние веток. Мердж ветки new_branch в master (в мастере нет новых комитов)
``` bash
# работа в new_branch завершина, все закомичено
git checkout master                    # сначала перейти в мастер
git merge new_branch                   # вмерджить ветку new_branch в мастер
# указатель мастера просто переходит на new_branch
git branch -d new_branch               # удалить ветку new_branch (если её изменения уже влиты в главную ветку)
```



#### Слияние веток. Pull Request
``` bash
Pull Request — это запрос на вливание изменений из вашей ветки в основную ветку исходного репозитория.
Чтобы создать Pull Request, зайдём на страницу вашего форка на github.com 
Справа от выпадающего меню с выбором ветки есть кнопка «New pull request».
Вы попадаете в окно сравнения веток.
Видим изменения — все верно, то нажимайте кнопку «Create pull request»
После нажатия кнопки появится окно ввода сообщения Pull Request. Сообщение PR — это описание того, что сделано и зачем (высокоуровневое описание).
Нажимаем «Create pull request».
---  начинается рецензирование изменений (code review) -----
```
 


 
