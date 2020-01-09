# Kazauwa_microservices

## Table of contents

1. [Docker](https://github.com/Otus-DevOps-2019-08/Kazauwa_microservices/tree/master/wiki/docker.md)

## Gitlab

### Установка

Gitlab позволяет развернуть собственный инстанс приложения. Это можно сделать, например, с помощью докера:

```bash
# mkdir -p /srv/gitlab/config /srv/gitlab/data /srv/gitlab/logs
# docker-compose -f gitlab-ci/docker-compose.yml up -d
```

Приложение поднимается несколько минут, проследить за процессом можно следующим образом:

```bash
# docker logs -f gitlab
```

### CI/CD

Для корректной работы CI/CD пайплайнов необходимо описать сами пайплайны и завести раннера, который будет их выполнять.

Начнём с раннера. Предположим, что нужный репозиторий уже создан. В панели слева нужно перейти в Settings > CI/CD > Runners settings и скопировать оттуда токен, он понадобится для регистрации раннера. Сам раннер можно установить либо через докер, либо через пакетный менеджер:

```bash
# docker
docker run -d --name gitlab-runner --restart always \
           -v /srv/gitlab-runner/config:/etc/gitlab-runner \
           -v /var/run/docker.sock:/var/run/docker.sock \
           gitlab/gitlab-runner:latest

# пакетный менеджер (apt, yum, dnf и т.п.)
apt install gitlab-runner
```

Регистрация раннера:

```bash
docker exec -it gitlab-runner gitlab-runner register --run-untagged --locked=false
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://<IP>/
Please enter the gitlab-ci token for this runner:
<TOKEN>
Please enter the gitlab-ci description for this runner:
[38689f5588fe]: my-runner
Please enter the gitlab-ci tags for this runner (comma separated):
linux,xenial,ubuntu,
Please enter the executor:
docker
Please enter the default Docker image (e.g. ruby:2.1):
alpine:latest
Runner registered successfully.
```

Пример описания пайплайна можно посмотреть в файле `.gitlab-ci.yml`. Его необходимо закоммитить в корень репозитория.

### Окружения

Гитлаб позволяет отнести отдельные джобы пайплайна к разным окружениям, например на staging и production. Сделать это очень просто — достаточно добавить в джобу в `gitlab-ci.yml` такой блок:

```yml
environment:
  name: production
  url: http://example.com
```

Гитлаб автоматически создаст окружение, после того как джоба с окружением выполнится в первый раз.
