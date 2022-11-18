# YaMDb

![YaMDb workflow](https://github.com/Irko-T/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Описание проекта

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором.

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

Произведению может быть присвоен жанр (Genre) из списка предустановленных. Новые жанры может создавать только администратор.

Пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.

&ensp;

## Как запустить проект (рекомендация для MacOS)

 Клонировать репозиторий и перейти в него в командной строке:

``` bash
git clone git@github.com:Irko-T/infra_sp2.git
```

``` bash
cd infra
```

Запуск контейнеров:

``` bash
docker-compose up -d --build
```

Выполнение миграции:

``` bash
docker-compose exec web python manage.py migrate
```

Создание суперпользователя

``` bash
docker-compose exec web python manage.py createsuperuser
```

Сборка статитки:

``` bash
docker-compose exec web python manage.py collectstatic --no-input
```

&ensp;

## Шаблон наполнения env-файла

``` bash
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```

&ensp;

## Примеры запросов к API

### Регистрация нового пользователя. Код подтвержения отправляется на переданный email

POST `api/v1/auth/signup/`

``` JSON
{
    "username": "test_user",
    "email": "test@mail.ru"
}
```

Ответ:

``` JSON
{
    "email": "test@mail.ru",
    "username": "test_user"
}
```

&nbsp;

### Получение JWT-токена

POST `api/v1/auth/token/`

``` JSON
{
    "username": "test_user",
    "confirmation_code": "639-c134f9e28fb1ada9c58a"
}
```

Пример ответа:

``` JSON
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjYwMzg1NzIzLCJqdGkiOiIyODliNDM1MTA1YjQ0MDNiOTE3ZTViM2U0NjQwMGE2OSIsInVzZXJfaWQiOjV9.PX9SWNxRi-bAGIPzc3p_PI8l565SrJIuzCLZxYyo"
}
```

&nbsp;

### Получение списка всех категорий

GET  `api/v1/categories/`

Пример ответа:

``` JSON
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "name": "books",
            "slug": "2"
        },
        {
            "name": "movies",
            "slug": "1"
        }
    ]
}
```

&nbsp;

### Получение информации о произведении

GET `api/v1/titles/{titles_id}/`

Пример ответа:

``` JSON
{
    "id": 2,
    "name": "Gone Girl",
    "year": 2014,
    "description": "",
    "genre": [],
    "category": {
        "name": "movies",
        "slug": "1"
    },
    "rating": 9
}
```
