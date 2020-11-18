# bsl-actions-template

Шаблон нового проекта 1С для автоматизации на GitHub Actions

## Ограничения

Работает на собственном [github runner](https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners). 
Предварительно его нужно настроить. Устанавливаем:
* Платформу 1С нужной версии. Например 8.3.17.1846
* OneScript 1.4
* Пакеты OneScript:
    - add
    - vanessa-runner
## Автоматизация

Для автоматизации рутинных и скучных действий используется GitHub Actions. 

Автоматизированы процессы:

* Cинтаксический контроль
* Модульное тестирование
* Публикация отчета о тестировании Allure
* Статический анализ SonarQube
* Публикация cf файла в релиз

## Секреты

Перед началом запуска workflow нужно настроить секреты в проекте (Settings -> Secrets). Нужны следующие переменные:
* SONARQUBE_HOST - ссылка на сервис SonarQube. Например: https://open.checkbsl.org/.
* SONARQUBE_TOKEN - токен доступа на сервис SonarQube. Генерируется на странице профиля в SonarQube.

## Настройка GitHub Pages для Allure

Для публикации отчетов Allure используется GitHub Pages. Предварительно нужно создать новую пустую ветку `gh-pages`.

## Блок CI

Блок CI описан в файле [ci.yml](/.github/workflows/ci.yml). 

### Предварительная настройка

1. Изменить в файле переменную среды `BASE_PATH`.
    ```
    BASE_PATH: ./bsl-actions-template
    ```
    Меняем `./bsl-actions-template` на каталог проекта GitHub. Например `./my-project`. 

2. Условие `if: github.repository == 'silverbulleters/bsl-actions-template'` в задании `sonar-analyze` меняем на условие с "путем" к проекту. 
Например: `if: github.repository == 'author/my-project`.

### Действия

В workflow 3 задачи:
* `testing` - сборка и тестирования проекта с помощью vanessa-runner и xUnit (из ADD)
* `sonar-analyze` - анализ проекта 1с на SonarQube
* `allure` - публикация отчеты Allure на странице GitHub Pages. Пример адреса: https://silverbulleters.github.io/bsl-actions-template

## Блок CD

Часть блока CD описано в файле [release.yml](/.github/workflows/release.yml). Как и в блоке CI нужно поменять значение у 
переменной среды `BASE_PATH`.

В итоге при создании нового релиза автоматически будет собрана CF и опубликована в релизе.