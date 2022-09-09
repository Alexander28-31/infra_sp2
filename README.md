# API YaMDb

## API интернет-сервиса YaMDB для хранения рецензий на произведения

### Запуск приложения в контейнерах

Сначала нужно клонировать репозиторий и перейти в корневую папку:
```
git clone git@github.com:Alexander28-31/infra_sp2.git
cd infra_sp2
```

Затем нужно перейти в папку infra_sp2/infra и создать в ней файл .env с 
переменными окружения, необходимыми для работы приложения.
```
cd infra/
```

Пример содержимого файла:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY=key
```

Далее следует запустить docker-compose: 
```
docker-compose up -d
```
Будут созданы и запущены в фоновом режиме необходимые для работы приложения 
контейнеры (db, web, nginx).

Затем нужно внутри контейнера web выполнить миграции, создать 
суперпользователя и собрать статику:
```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
```
После этого проект должен быть доступен по адресу http://localhost/.


### Заполнение базы данных

Нужно зайти на на http://localhost/admin/, авторизоваться и внести записи 
в базу данных через админку.

Резервную копию базы данных можно создать командой
```
docker-compose exec web python manage.py dumpdata > fixtures.json 
```

### Остановка контейнеров

Для остановки работы приложения можно набрать в терминале команду Ctrl+C 
либо открыть второй терминал и воспользоваться командой
```
docker-compose stop 
```
Также можно запустить контейнеры без их создания заново командой
```
docker-compose start 
```

### Документация в формате Redoc:

Чтобы посмотреть документацию API в формате Redoc, нужно локально запустить 
проект и перейти на страницу http://127.0.0.1:8000/redoc/