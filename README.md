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

### Регистрация новых пользователей
1. Пользователь отправляет POST-запрос с параметрами `email` и `username` на эндпоинт `/api/v1/auth/signup/`.
```
{
"email": "newuser@example.com",
"username": "newuser"
}
```
2. Сервис **YaMDB** отправляет письмо с кодом подтверждения (`confirmation_code`) на указанный адрес `email`.
3. Пользователь отправляет POST-запрос с параметрами `username` и `confirmation_code` на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит `token` (JWT-токен).
```
{
"username": "newuser",
"confirmation_code": "bm19kk-19c5fbeb4f340e882e7c7c624ce4f2ce"
}
```
Пример ответа:
```
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjg4MjIyMDYzLCJpYXQiOjE2ODA0NDYwNjMsImp0aSI6IjQ4MTdlNjUwYjNiMjRkY2JhNmRhMjBiODE3OGEzMGQzIiwidXNlcl9pZCI6Nn0.ntkEoWCxmK_oj6zoOJxj2sCkPlQJ0BTdaZOTHN9vjJk"
}
```
### Примеры запросов
#### Получение информации о произведении
Отправьте GET-запрос на эндпоинт `/api/v1/titles/1/`.
Пример ответа:
```
{
    "id": 1,
    "name": "Побег из Шоушенка",
    "year": 1994,
    "rating": 10,
    "description": null,
    "genre": [
        {
            "name": "Драма",
            "slug": "drama"
        }
    ],
    "category": {
        "name": "Фильм",
        "slug": "movie"
    }
}
```
#### Добавление нового отзыва 
Отправьте POST-запрос на эндпоинт `/api/v1/titles/1/reviews/`, передав текст отзыва в поле `text` и оценку в поле `score`. 
```
{
"text": "Ставлю десять звёзд!",
"score": 10
}
```
Пример ответа:
```
{
    "id": 11,
    "text": "Ставлю десять звёзд!",
    "author": "newuser",
    "score": 10,
    "pub_date": "2023-04-02T15:51:58.834794Z"
}
```

### Автор
Галина Фишер, студент когорты 18+ Яндекс.Практикум
