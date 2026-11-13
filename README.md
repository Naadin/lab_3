# lab_3

## Часть 1. Запуск вручную (контейнеры `docker run`)

```bash
#создадим контейнер redis
docker run -d --name redis redis
```

![run_redis.png](run_redis.png)

Контейнер создан в фоновом режиме с именем `redis`. Образ `redis:latest` был автоматически скачан из Docker Hub.

```bash
#проверим, что контейнер создан
docker ps
```

![see_redis.png](see_redis.png)

Контейнер `redis` в статусе `Up`, слушает порт `6379/tcp`.

```bash
#создадим приложение rocketcounter
docker run -d --name rocketcounter --link redis:redis -p 8085:5000 rotorocloud/rocket-counter
```

![run_rocketcounter.png](run_rocketcounter.png)

Создаем приложение из образа `rotorocloud/rocket-counter`, связываем его с `Redis` через `--link` и публикуем на порту `8085`. Флаг `--link redis:redis` связывает контейнер `rocketcounter` с контейнером `redis`.

```bash
#проверим
docker ps
```

![see_two.png](see_two.png)

Зачем в Ч.1 использовался `--link` и почему в Compose он не нужен?
- `--link` исторически устарел. В этой части используем его намеренно, чтобы увидеть ручную связку. В Compose он не нужен.
При запуске `docker run`, у контейнеров нет общей связи. Чтобы один контейнер мог обратиться к другому по имени, как раз и применяли `--link`. Это нужно было для упрощения обращения между контейнерами.
В Compose он не нужен, потому что Docker Compose сам создает сеть для проекта и перемещает туда все сервисы, зависимости по порядку запуска оформляются через `depends_on`, а не через `--link`.

Проверим, что приложение отдаёт HTML-страницу: 

![see_curl.png](see_curl.png)

Откроем ее на `localhost`:

![get_1.png](get_1.png)

Понажимаем на кнопку `GET A ROCKET` и убедимся, что счетчик работает:

![get_2.png](get_2.png)

Счётчик растёт, значит приложение успешно работает и сохраняет данные в `Redis`.

```bash
#остановим контейнеры
docker stop rocketcounter redis
#удалим контейнеры
docker rm rocketcounter redis
#посмотрим
docker ps -a
```

![stop.png](stop.png)

Остались только старые контейнеры - все оки.

## Часть 2. Описание стека в `docker-compose.yml`

![]()

## Часть 3. Масштабирование приложения



## Часть 4. Полное уничтожение стека



## Вопросы
