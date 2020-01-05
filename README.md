# Kazauwa_microservices

## Table of contents

1. [Docker](https://github.com/Otus-DevOps-2019-08/Kazauwa_microservices/tree/master/wiki/docker.md)
2. [Gitlab](https://github.com/Otus-DevOps-2019-08/Kazauwa_microservices/tree/master/wiki/gitlab.md)

## Prometheus

### Установка

Самый простой способ запуска — докер:

```bash
# docker run --rm -p 9090:9090 -d --name prometheus prom/prometheus:v2.1.0
```

Веб-интерфейс будет доступен по адресу http:127.0.0.1:9090, однако пока что в нём нет ничего особо интересного. Прометей умеет мониторить сам себя, эти метрики идут из коробки и доступны всегда. Гораздо интереснее понаблюдать за рабочими приложениями, но для этого нужна дополнительная конфигурация.

### Конфигурация

Прометей настраивается при помощи флагов командной строки и/или конфигурационного файла. Для удобства, воспользуемся последним — `monitoring/prometheus/prometheus.yml`. В самом простом виде конфигурация выглядит так:

```yaml
---
global:
  scrape_interval: '5s'

scrape_configs:
  - job_name: 'myjob'
    static_configs:
      - targets:
        - '127.0.0.1:9090'
```

Разберём каждый параметр подробнее:

- `scrape_interval` — как часто прометей должен собирать метрики
- `job_name` — название джобы (читай, группы сервисов)
- `targets` — адреса для сбора метрик (указание порта обязательно)

Передать этот конфиг контейнеру с прометеем можно двумя способами: примонтировать в базовый образ или написать свой. Пример своего образа можно посмотреть в `monitoring/prometheus/Dockerfile`.

### Запуск

См. README в директории `src/`. TL;DR — заполнить `.env` и выполнить `docker-compose up -d`. Готовые образы можно найти на [докерхабе автора](https://hub.docker.com/u/kazauwa/).

### Что можно мониторить

Да что угодно. Для прометея написано много экспортеров, которые умеют собирать и отдавать метрики для самых разных целей, в том числе и для приложений, исходный код которых нельзя поменять. Несколько примеров:

- Node exporter — собирает метрики с хостовой машины: нагрузку по ЦПУ, оперативную память, файловую систему и т.п.
- Blackbox exporter — позволяет проверять доступность сервисов с целевого хоста
- Postgres exporter — собирает метрики инстанса PostgreSQL
- Docker exporter — такого на самом деле нет, но докер умеет из коробки отдавать свои метрики в формате прометея

При разработке собственных приложений, можно воспользоваться [библиотеками](https://prometheus.io/docs/instrumenting/clientlibs/), которые реализуют клиент прометея.
