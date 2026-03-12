# API для Yatube

REST API для социальной сети Yatube - платформы для публикации постов и комментариев.

## Описание

API для Yatube предоставляет полный функционал для работы с контентом социальной сети:
- Публикация и управление постами
- Комментирование постов
- Работа с группами (сообществами)
- Система подписок на авторов
- JWT-аутентификация

API документирован с использованием OpenAPI спецификации. Интерактивная документация доступна по адресу `/redoc/` после запуска сервера.

## Технологии

- Python 3.9+
- Django 3.2.16
- Django REST Framework 3.12.4
- djangorestframework-simplejwt 4.7.2 (JWT аутентификация)
- django-filter 2.4.0 (фильтрация данных)
- Pillow 9.3.0 (работа с изображениями)

## Установка

1. Клонируйте репозиторий:
```bash
git clone <repository_url>
cd api-final-yatube-ad
```

2. Создайте и активируйте виртуальное окружение:

**Для macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

**Для Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

3. Установите зависимости:
```bash
pip install -r requirements.txt
```

4. Перейдите в директорию проекта:
```bash
cd yatube_api
```

5. Выполните миграции:
```bash
python manage.py migrate
```

6. (Опционально) Создайте суперпользователя:
```bash
python manage.py createsuperuser
```

7. Запустите сервер разработки:
```bash
python manage.py runserver
```

После запуска сервер будет доступен по адресу `http://127.0.0.1:8000/`

## Документация

Интерактивная документация API доступна по адресу:
- `http://127.0.0.1:8000/redoc/` - документация в формате ReDoc

## Примеры использования API

### Аутентификация

Получение JWT-токена:
```bash
POST http://127.0.0.1:8000/api/v1/jwt/create/
Content-Type: application/json

{
    "username": "your_username",
    "password": "your_password"
}
```

Ответ:
```json
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc...",
    "access": "eyJ0eXAiOiJKV1QiLCJhbGc..."
}
```

Использование токена в заголовках запросов:
```bash
Authorization: Bearer <your_access_token>
```

### Публикации

Получить список всех постов:
```bash
GET http://127.0.0.1:8000/api/v1/posts/
```

Создать новый пост (требуется аутентификация):
```bash
POST http://127.0.0.1:8000/api/v1/posts/
Authorization: Bearer <your_access_token>
Content-Type: application/json

{
    "text": "Текст поста",
    "group": 1
}
```

Получить конкретный пост:
```bash
GET http://127.0.0.1:8000/api/v1/posts/1/
```

Обновить пост (только автор):
```bash
PATCH http://127.0.0.1:8000/api/v1/posts/1/
Authorization: Bearer <your_access_token>
Content-Type: application/json

{
    "text": "Обновленный текст поста"
}
```

Удалить пост (только автор):
```bash
DELETE http://127.0.0.1:8000/api/v1/posts/1/
Authorization: Bearer <your_access_token>
```

### Комментарии

Получить комментарии к посту:
```bash
GET http://127.0.0.1:8000/api/v1/posts/1/comments/
```

Добавить комментарий (требуется аутентификация):
```bash
POST http://127.0.0.1:8000/api/v1/posts/1/comments/
Authorization: Bearer <your_access_token>
Content-Type: application/json

{
    "text": "Текст комментария"
}
```

### Группы

Получить список всех групп:
```bash
GET http://127.0.0.1:8000/api/v1/groups/
```

Получить информацию о группе:
```bash
GET http://127.0.0.1:8000/api/v1/groups/1/
```

### Подписки

Получить список подписок текущего пользователя (требуется аутентификация):
```bash
GET http://127.0.0.1:8000/api/v1/follow/
Authorization: Bearer <your_access_token>
```

Поиск по подпискам:
```bash
GET http://127.0.0.1:8000/api/v1/follow/?search=username
Authorization: Bearer <your_access_token>
```

Подписаться на пользователя (требуется аутентификация):
```bash
POST http://127.0.0.1:8000/api/v1/follow/
Authorization: Bearer <your_access_token>
Content-Type: application/json

{
    "following": "username_to_follow"
}
```

## Права доступа

- **Неаутентифицированные пользователи**: имеют доступ только на чтение (GET-запросы). Исключение - эндпоинт `/api/v1/follow/`, который доступен только аутентифицированным пользователям.

- **Аутентифицированные пользователи**:
  - Могут создавать посты и комментарии
  - Могут изменять и удалять только свой контент
  - Могут подписываться на других пользователей
  - Полный доступ к списку своих подписок

## Пагинация

Список постов поддерживает пагинацию через параметры:
- `limit` - количество постов на странице
- `offset` - смещение для пагинации

Пример:
```bash
GET http://127.0.0.1:8000/api/v1/posts/?limit=10&offset=20
```

## Фильтрация

Список постов можно фильтровать по группе:
```bash
GET http://127.0.0.1:8000/api/v1/posts/?group=1
```

## Тестирование

Для запуска тестов используйте pytest:
```bash
pytest
```

## Автор

Проект выполнен в рамках финального задания курса.
