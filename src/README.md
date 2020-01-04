# Приложение reddit

### Запуск

Заполнить файл `.env.example` и убрать окончание `.example`, затем выполнить:

```bash
# docker-compose up -d
```

И перейти по адресу `http://127.0.0.1:9292`

### Переменные окружения

- `USERNAME` — никнейм пользователя в docker registry
- `UI_VERSION` — версия приложения ui
- `PUBLISH_PORT` — порт хоста, на котором будет опубликовано приложение
- `POST_VERSION` — версия приложения post
- `COMMENT_VERSION` — версия приложения comment
