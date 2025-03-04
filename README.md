# Owncloud with local database
Это конфигурация для развертывания облачного хранилища ownCloud с использованием Docker Compose и локальной базой данных
Сервис owncloud предоставляет функциональность облачного хранилища, а redis используется для кэширования и сессий. Nginx используется как обратный прокси-сервер для обеспечения HTTPS и маршрутизации запросов к сервису owncloud.

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
git clone https://github.com/A3th3rS3t/Owncloud.git
cd Owncloud
```
В репозитории должны быть файлы `docker-compose.yaml`, `nginx.conf` и `.env`.

## 3. Настройка переменных
В местах, где есть комментарии, замените значения на свои. Например, в.env:
>OWNCLOUD_DB_PASSWORD=password   # change for your own password
>
>OWNCLOUD_DB_HOST=localhost      # set to your own database host if different

Такие строки с комментариями находятся в каждом из трех файлов.

## 4.  Настройка Nginx и SSL для Nginx (опционально, если вы хотите использовать HTTPS)
Убедитесь, что Nginx установлен на вашем сервере. Если нет, установите его: https://nginx.org/ru/docs/install/

Переместите файл nginx.conf в директорию конфигураций Nginx:
```bash
mv nginx.conf /etc/nginx/conf.d/
```
Проверьте конфигурацию Nginx:
```bash
nginx -t
```
Если проверка прошла успешно, перезапустите Nginx:
```bash
systemctl restart nginx
```

Убедитесь, что у вас есть SSL-сертификаты для вашего домена (В качестве альтернативного варианта получения SSL-сертификата можно использовать `Let's Encrypt`). Измените пути в nginx.conf, там где есть комментарии.

Разместите сертификаты по пути, указанному в nginx.conf:
```nginx
/etc/nginx/ssl/your_domain.ru/fullchain.pem
/etc/nginx/ssl/your_domain.ru/privkey.pem
```
> где `your_domain.ru` замените на ваш домен

## 5. Запуск сервисов через Docker Compose
В директории с проектом выполните команду для запуска сервисов:

```bash
docker-compose up -d
```
Это запустит два контейнера:
- `owncloud_app` — сервис ownCloud.
- `owncloud_redis` — сервис Redis для кэширования.

## 6. Проверка работоспособности
Откройте браузер и перейдите по адресу:
localhost:8080  (или другой порт, если вы изменили его в.env)

Если используется домен с SSL, перейдите по адресу: https://your_domain.ru

Вы должны увидеть интерфейс ownCloud.

Если что-то не работает, проверьте логи контейнеров:
```bash
docker logs owncloud_app
```

## 7. Остановка и удаление сервисов
Если вам нужно изменить порты или другие параметры, отредактируйте файл `docker-compose.yaml` и перезапустите сервисы:
```bash
docker-compose down
docker-compose up -d
```

## 8. Резервное копирование
Для резервного копирования данных ownCloud и Redis, сохраните содержимое томов:

`/owncloud-data` — данные ownCloud.

`./volumes/redis` — данные Redis.

## Postscriptum
Иногда требуется вносить правки в конфигурацию ownCloud по пути `/owncloud-data/config/config.php` и перезапускать сервисы, это требуется для корректной работы доменов и приложения
