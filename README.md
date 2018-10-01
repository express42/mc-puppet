# Мастер класс DevOpsConf
## Современные подходы к разработке инфраструктурного кода на Puppet 

Установливаем Puppet DK. 
Ссылка на доступные пакеты https://puppet.com/download-puppet-development-kit, варианты установки перечислены по ссылке https://puppet.com/docs/pdk/1.x/pdk_install.html
Создаем новый модуль:
```
pdk new module pdk_module
```
Переходим в директорию модуля и выводим структуру:
```
cd pdk_module/
ls -la
```
Просматриваем сгенерированую метадату модуля:
```
cat metadata.json
```
Инициализируем git репозиторий и делаем первый коммит:
```
git init
git add .
git commit -m "Create new module pdk_module from template"
```
Создаем init класс:
```
pdk new class pdk_module
```
Создаем еще классы:
```
pdk new class package
pdk new class service
```
Создаем task:
```
pdk new task restart
```
Создаем defined_type:
```
pdk new defined_type defined_type
```
Пробуем создать новый provider:
```
pdk new provider provider
```
Получаем ошибку, переходим по ссылке в ошибке и подключаем gem puppet-resource_api по инструкции.

Проверяем, что поменялось после выполнения pdk update:
```
cat update_report.txt
```
Пробуем создать provider еще раз:
```
pdk new provider provider
```
Все, структура модуля готова. Комитим изменения:
```
git add .
git commit -m "Add new classes, task, provider and defined_type"
```
Далее переходим к тестированию модуля.

Выводим список доступных валидаторов кода:
```
pdk validate --list
```
Выполняем валидацию кода:
```
pdk validate
```
Попробуем выполнить валидацию кода с другой версией Puppet:
```
pdk validate --puppet-version=4
```
Удаляем дефолтные настройки .rubocop.yml
```
rm .rubocop.yml
```
Пробуем провалидировать код:
```
pdk validate
```
Восстанавливаем удаленные настройки:
```
pdk update
```
Выполняем юнит тестирование модуля:
```
pdk test unit
```
Пробуем выполнить rubocop через pdk bundle exec:
```
pdk bundle exec rubocop
```
Выводим список всех rake задач:
```
pdk bundle exec rake
```
Увеличиваем версию модуля:
```
pdk bundle exec rake module:bump
```
Собираем пакет:
```
pdk build
```
Выводим список собранных пакетов:
```
ls -la pkg/
```
Пробуем добавить модуль на Puppet Forge:
```
pdk bundle exec rake module:push
```
Регистрируемся на Puppet Forge и указаваем параметры для входа:
```
vi ~/.puppetforge.yml
```
Пробуем еще раз:
```
pdk bundle exec rake module:push
```
Создаем репозиторий на GitHub и подключаем его к нашему репозиторию:
```
git remote add origin ...
git push -u origin master
```
Выполняем релиз модуля:
```
pdk bundle exec rake module:release
```
Пробуем сгененировать CHANGELOG:
```
pdk bundle exec rake changelog
```
Генерируем GitHub токен и экспортируем токен как переменную:
```
export CHANGELOG_GITHUB_TOKEN=valid_token_here
```
Генерируем CHANGELOG:
```
pdk bundle exec rake changelog
```

## Домашнее задание
- Попробовать pdk convert на своих модулях;

- Протестировать модуль в Beaker https://github.com/puppetlabs/beaker-rspec;

- Выполнить тестирование модуля через Gitlab или Travis CI;

- Склонировать control repo https://github.com/puppetlabs/control-repo.git, подключить модули через Puppetfile, установить R10k и залить изменения на Puppet Master.

