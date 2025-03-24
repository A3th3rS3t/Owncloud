# Owncloud
Это конфигурация для развертывания облачного хранилища ownCloud с использованием Docker Compose и локальной базой данных
Сервис owncloud предоставляет функциональность облачного хранилища, а redis используется для кэширования и сессий.

## 1. Подготовка окружения
Убедитесь, что у вас установлены `Docker` и `Docker Compose`, если нет воспользуйтесь следующей инструкцией: https://docs.docker.com/engine/install/

Убедитесь, что `Git` установлен. Если нет, установите его: https://git-scm.com/downloads

Убедитесь, что у вас установлена База данных MySQL, PostgreSQL или MariaDB. А так же созданна база данных с именем `owncloud` и пользователем `owncloud` с паролем.

## 2. Клонирование репозитория
Перейдите в директорию, в которой вы хотите развернуть ownCloud:
```bash
cd /opt/docker/
```

Клонируйте репозиторий с GitHub:
```bash
git clone https://github.com/A3th3rS3t/Owncloud-docker.git
cd Owncloud
```
В репозитории должны быть файлы `docker-compose.yaml` и `.env`.

## 3. Настройка переменных
В местах, где есть комментарии, замените значения на свои. Например, в.env:
>OWNCLOUD_DB_PASSWORD=password   # change for your own password
>
>OWNCLOUD_DB_HOST=localhost      # set to your own database host if different

Такие строки с комментариями находятся в каждом из трех файлов.

## 4. Запуск сервисов через Docker Compose
В директории с проектом выполните команду для запуска сервисов:

```bash
docker-compose up -d
```
Это запустит два контейнера:
- `owncloud_app` — сервис ownCloud.
- `owncloud_redis` — сервис Redis для кэширования.

## 5. Проверка работоспособности
Откройте браузер и перейдите по адресу:
localhost:8080  (или другой порт, если вы изменили его в.env)

Если что-то не работает, проверьте логи контейнеров:
```bash
docker logs owncloud_app
```

## 6. Остановка и удаление сервисов
Если вам нужно изменить порты или другие параметры, отредактируйте файл `docker-compose.yaml` и перезапустите сервисы:
```bash
docker-compose down
docker-compose up -d
```

## 7. Резервное копирование
Для резервного копирования данных ownCloud и Redis, сохраните содержимое томов:

`/owncloud-data` — данные ownCloud.

`./volumes/redis` — данные Redis.

## Postscriptum
Иногда требуется вносить правки в конфигурацию ownCloud по пути `/owncloud-data/config/config.php` и перезапускать сервисы, это требуется для корректной работы доменов и приложения
