# Kazauwa_microservices

## Приложение reddit

### Установка

Создать образ для каждого из микросервисов:

```bash
# docker build -f src/post-py/Dockerfile -t reddit/post:latest src/post-py
# docker build -f src/comment/Dockerfile -t reddit/comment:latest src/comment
# docker build -f src/ui/Dockerfile -t reddit/ui:latest src/ui
```

Скачать образ базы данных:

```bash
# docker pull mongo:latest
```

Создать сеть, через которую контейнеры будут общаться:

```bash
# docker network create reddit
```

Опционально, можно создать персистентный вольюм для базы данных, чтобы данные не пропадали между перезапусками контейнеров:

```bash
# docker volume create reddit_db
```

### Запуск

Выполнить следующие команды:

```bash
# docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
# docker run -d --network=reddit --network-alias=post reddit/post:latest
# docker run -d --network=reddit --network-alias=comment reddit/comment:latest
# docker run -d --network=reddit -p 9292:9292 reddit/ui:latest
```

И перейти по адресу `http://127.0.0.1:9292`
