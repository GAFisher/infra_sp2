# Проект YaMDb
### Описание
Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории, такие как «Книги», «Фильмы», «Музыка». 

Документация будет доступна по адресу `http://localhost/redoc/` после того, как вы запустите проект. В документации описано, как должен работать ваш API. Документация представлена в формате Redoc.

### Как запустить проект
1. Создайте и активируйте виртуальное окружение
```
python3 -m venv env
source env/bin/activate
```
2. Установите зависимости из файла requirements.txt
```
python3 -m pip install --upgrade pip
pip install -r requirements.txt
```
3. Создайте файл _.env_ с переменными окружения внутри директории infra (на одном уровне с docker-compose.yaml).

    Шаблон наполнения env-файла:
```
SECRET_KEY = 'p&l%385148kslhtyn^##a1)ilz@4zqj=rq&agdol^##zgl9(vs'
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```
4. Запустите docker-compose командой `docker-compose up -d --build`.

5. Выполните по очереди команды:
```
docker-compose exec web python manage.py makemigrations user
docker-compose exec web python manage.py makemigrations reviews
docker-compose exec web python manage.py migrate
```
* Выполните команду для заполнения базы данными:  
```
docker-compose exec web python manage.py import_csv_to_orm
```
* Создайте суперпользователя: 
```
docker-compose exec web python manage.py createsuperuser
```
* Выполните команду для сбора статических файлов:
```
docker-compose exec web python manage.py collectstatic --no-input
```
* Создайте дамп (резервную копию) базы:
```
docker-compose exec web python manage.py dumpdata > fixtures.json 
```
6. Проект будет доступен по адресу `http://localhost/`.

### Автор
Галина Фишер, студент когорты 18+ Яндекс.Практикум
