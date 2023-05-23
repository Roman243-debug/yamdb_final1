# CI и CD проекта api_yamdb

![workflow](https://github.com/Roman243-debug/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

В проекте yamdb_final произведена настройка для приложения api_yamdb Continuous Integration и Continuous Deployment:
- автоматический запуск тестов,
- обновление образов на Docker Hub,
- автоматический деплой на боевой сервер при пуше в главную ветку master.
---
Проект реализует API для сервиса YaMDb — базы отзывов о фильмах, книгах и музыке.

### Технологии
Python 3.7
Django REST framework 3.12.4

### Установка проекта

#### Клонируйте данный репозиторий
```git clone https://github.com/Roman243-debug/yamdb_final```


### Установка приложения docker на севере

#### Установите docker на сервер:
```
sudo apt install docker.io 
```
#### Установите docker-compose на сервер:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
#### Примените разрешения для исполняемого файла к двоичному файлу:
```
sudo chmod +x /usr/local/bin/docker-compose
```

#### Отредактируйте файл nginx/default.conf и в строке server_name впишите свой IP

#### Скопируйте файлы docker-compose.yaml и nginx/default.conf из проекта на сервер:
```
scp docker-compose.yaml <username>@<host>/home/<username>/docker-compose.yaml
scp default.conf <username>@<host>/home/<username>/nginx/default.conf
```

#### Добавьте в Secrets GitHub переменные окружения для работы:
```
SECRET_KEY=<secret key django проекта>
DB_ENGINE=django.db.backends.postgresql
DB_HOST=db
DB_NAME=postgres
DB_PASSWORD=postgres
DB_PORT=5432
DB_USER=postgres

DOCKER_PASSWORD=<Docker password>
DOCKER_USERNAME=<Docker username>

USER=<username для подключения к серверу>
HOST=<IP сервера>
PASSPHRASE=<пароль для сервера, если он установлен>
SSH_KEY=<ваш SSH ключ(cat ~/.ssh/id_rsa)>

TG_CHAT_ID=<ID чата, в который придет сообщение>
TELEGRAM_TOKEN=<токен вашего бота>
```

### После успешного деплоя зайдите на боевой сервер и выполните команды (только после первого деплоя):
#### Создайте суперпользователя
```
docker-compose exec web python manage.py createsuperuser
=======
```
#### Импортируйте демострационные данные
```
docker-compose exec web python manage.py import_data  
```
### Сервер и документация по API

Адрес приложения: http://{ваш IP}/api/v1/

Документация: http://{ваш IP}/redoc/

Тестовый сервер:
http://130.193.40.60/api/v1/
http://130.193.40.60/redoc/
http://130.193.40.60/admin/
