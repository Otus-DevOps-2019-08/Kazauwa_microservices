# Kazauwa_microservices

## Docker

### Как использовать

Для создания монолитного образа с приложением reddit и mongodb, в корне этого репозитория необходимо выполнить такую команду:

```bash
$ docker build -t reddit:latest ./docker-monolith
```

Докер последовательно выполнит все инструкции описанные в Dockerfile и создаст образ с тегом `reddit:latest`. Для запуска контейнера из этого образа:

```bash
$ docker run --rm -d --name reddit --network=host reddit:latest
```

Результат можно посмотреть, перейдя по `http://127.0.0.1:9292`.

**Важно!**
Опцию `--network=host` обычно не рекомендуется использовать, так как нарушается сетевая изоляция между докером и хостовой машиной. В данном примере эта опция нужна, чтобы не переусложнять вводное задание.

Теперь, можно "залезть" внутрь контейнера и посмотреть на его файловую систему:

```bash
$ docker exec -ti reddit /bin/bash
```

Например, посмотреть на логи mongodb: `less /var/log/mongod.log`.


### Docker Machine
Этот инструмент позволяет локально управлять удалённым хостом. Для примера, можно создать инстанс в GCP, установить на нём докер и управлять им со своей локальной машины:

```bash
$ docker-machine create --driver google \
                        --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts \
                        --google-machine-type n1-standard-1 \
                        --google-zone europe-west2-c \
                        docker-host
```

Для активации этой машины:

```bash
$ eval $(docker env docker-host)
```

Теперь, команда `docker` управляет хостом в GCP. Для проверки, можно выполнить команду `docker images` и убедиться, что список будет пустым. Несмотря на удалённость, мы всё равно можем собирать образы из локальных файлов: `docker-machine` отправит весь необходимый контекст по сети.

Для переключения на локальный докер:

```bash
$ eval $(docker env --unset)
```
