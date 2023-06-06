# Проект YaMDb

Проект **YaMDb** собирает отзывы пользователей на произведения. Сами произведения в **YaMDb** не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

Проект Запущен на Этом IP: **158.160.99.141**
 
## Workflow
![Workflow](https://github.com/EG0RIAN/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
* tests - Проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8) и запуск pytest. Дальнейшие шаги выполнятся только если push был в ветку master или main.
* build_and_push_to_docker_hub - Сборка и доставка докер-образов на Docker Hub
* deploy - Автоматический деплой проекта на боевой сервер. Выполняется копирование файлов из репозитория на сервер:
* send_message - Отправка уведомления в Telegram

### Подготовка для запуска workflow
Создайте и активируйте виртуальное окружение, обновите pip:
```
python3 -m venv venv
. venv/bin/activate
python3 -m pip install --upgrade pip
```
Запустите автотесты:
```
pytest
```
Отредактируйте файл `nginx/default.conf` и в строке `server_name` впишите IP виртуальной машины (сервера).  
Скопируйте подготовленные файлы `docker-compose.yaml` и `nginx/default.conf` из вашего проекта на сервер:

Зайдите в репозиторий на локальной машине и отправьте файлы на сервер.
Можно сделать 2умя способами, первый склонировав репозиторий, переместив нужные файлы командой mv
после чего удалить остаток 
```
rm -rf hw05_final
```
Или 2ой вариант:
```
scp docker-compose.yaml <username>@<host>/home/<username>/docker-compose.yaml
sudo mkdir nginx
scp default.conf <username>@<host>/home/<username>/nginx/default.conf
```
В репозитории на Гитхабе добавьте данные в `Settings - Secrets - Actions secrets`:
```
DOCKER_USERNAME - имя пользователя в DockerHub
DOCKER_PASSWORD - пароль пользователя в DockerHub
HOST - ip-адрес сервера
USER - пользователь
SSH_KEY - приватный ssh-ключ (публичный должен быть на сервере)
PASSPHRASE - кодовая фраза для ssh-ключа
DB_ENGINE - django.db.backends.postgresql
DB_HOST - db
DB_PORT - 5432
TELEGRAM_TO - id своего телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN - токен бота (получить токен можно у @BotFather, /token, имя бота)
DB_NAME - postgres (по умолчанию)
POSTGRES_USER - postgres (по умолчанию)
POSTGRES_PASSWORD - postgres (по умолчанию)
```



## Как запустить проект на сервере:


Установите Docker и Docker-compose:
```
sudo apt install docker.io
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
Проверьте корректность установки Docker-compose:
```
sudo  docker-compose --version
```
Создайте папку `nginx`:
```
mkdir nginx
```
### После успешного деплоя:
Соберите статические файлы (статику):
```
docker-compose exec web python manage.py collectstatic --no-input
```
Примените миграции:
```
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate --noinput
```
Создайте суперпользователя:
```
docker-compose exec web python manage.py createsuperuser

```
или
```
docker-compose exec web python manage.py loaddata fixtures.json
```
# Как запустить проект:
Клонировать репозиторий и перейти в него в командной строке:

`git clone https://github.com/EG0RIAN/api_yamdb.git`

`cd api_yamdb`

Создать и активировать виртуальное окружение:

`python -m venv venv`

`source venv/bin/activate`

Установить зависимости из файла requirements.txt:

`python -m pip install --upgrade pip`

`pip install -r requirements.txt`

Выполнить миграции:

`python manage.py migrate`

Запустить проект:

`python manage.py runserver`

# Где Можно Посмотреть на примеры запросов к API ?
Например, после запуска проекта можно посмотреть [Документацию](http://127.0.0.1:8000/redoc/).

# Основные запросы
POST http://127.0.0.1:8000/api/v1/auth/signup/  
<sub>Регистрация Пользователя </sub>

    {
        "username":"Fake",
        "email": "Fake.fake@gmail.com"
    }

POST http://127.0.0.1:8000/api/v1/auth/token/        
в бэк приходит письмо с кодом  
<sub>Получение Jwt Токена</sub>

    {
        "username":"Fake",
        "confirmation_code": "biv4hr-69c31c5b2ad53627baf8c1f5273156e3"
    }

POST http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/1/ 
<sub>Оставить комментарий для</sub>  
<sub>произведения с id=1</sub>  
<sub>ревью id=1</sub>  
<sub>Коммента id=1</sub>  

    {
        "text": "testComment"
    }

POST http://127.0.0.1:8000/api/v1/auth/users/me/
в бэк приходит письмо с кодом  
<sub>Получение Jwt Токена</sub>

    {
        "username": "Fake",
        "email": "Fake.fake@fake.com",
        "first_name": "",
        "last_name": "",
        "bio": "",
        "role": "user"
    }

# Над проектом работали

[Егор Архипцев](https://github.com/EG0RIAN)<br>
[Григорий Кононихин](https://github.com/ddk-ops)<br>
[Роман Демчак](https://github.com/JuniorRF/)

