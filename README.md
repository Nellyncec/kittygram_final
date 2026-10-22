![Main Kittygram workflow](https://github.com/Nellyncec/kittygram_final/actions/workflows/main.yml/badge.svg)

# Kittygram

Социальная сеть для владельцев котиков: можно добавить котика с фото,
цветом, годом рождения и достижениями.

Проект запускается в Docker-контейнерах. При пуше в `main` GitHub Actions
проверяет бэкенд линтером ruff, запускает тесты бэкенда и фронтенда, собирает
образы и публикует их на Docker Hub, а затем присылает уведомление в Telegram.

## Стек

Python 3.9, Django 3.2, Django REST Framework, Djoser, PostgreSQL 13,
React, Nginx, Gunicorn, Docker, GitHub Actions.

## Контейнеры

| Сервис | Образ | Назначение |
|--------|-------|------------|
| `db` | `postgres:13` | база данных |
| `backend` | `nellyncec/kittygram_backend` | Django + Gunicorn |
| `frontend` | `nellyncec/kittygram_frontend` | собирает React и копирует сборку в volume `static` |
| `gateway` | `nellyncec/kittygram_gateway` | Nginx: раздаёт статику и медиа, проксирует `/api/` и `/admin/` |

Volumes: `pg_data` (данные PostgreSQL), `static` (статика фронтенда и
админки), `media` (загруженные фото).

## Локальный запуск

```bash
git clone git@github.com:Nellyncec/kittygram_final.git
cd kittygram_final
cp .env.example .env    # и задать свой SECRET_KEY
docker compose up --build
```

Миграции и сборка статики выполняются при старте контейнера `backend`.
Проект откроется на http://localhost:9000.

Создать суперпользователя для админки:

```bash
docker compose exec backend python manage.py createsuperuser
```

## Запуск из образов Docker Hub

```bash
docker compose -f docker-compose.production.yml up -d
```

## Переменные окружения

| Переменная | Назначение |
|------------|------------|
| `POSTGRES_DB` | имя базы |
| `POSTGRES_USER` | пользователь базы |
| `POSTGRES_PASSWORD` | пароль базы |
| `DB_HOST` | хост базы (`db` в compose) |
| `DB_PORT` | порт базы |
| `SECRET_KEY` | секретный ключ Django |
| `DEBUG` | режим отладки (`True`/`False`) |
| `ALLOWED_HOSTS` | разрешённые хосты через запятую |

## Секреты GitHub Actions

| Секрет | Значение |
|--------|----------|
| `DOCKER_USERNAME` | логин на Docker Hub |
| `DOCKER_PASSWORD` | токен доступа Docker Hub |
| `TELEGRAM_TOKEN` | токен Telegram-бота |
| `TELEGRAM_TO` | chat id получателя |
