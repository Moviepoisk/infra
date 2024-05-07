# Movies

[Техническое задание](https://practicum.yandex.ru/learn/middle-python-extended/courses/6632251a-381d-4f6d-afd5-f5deb39c5685/sprints/178804/topics/7b8cdb03-cfda-4146-b153-5789a64e43b9/lessons/fb200671-b523-4537-a7e8-93c1c2631f6e/)

## Структура

## Запуск приложения

### Все
Запустить сразу все микросервисы
```bash
docker compose up

docker compose -f docker-compose.yml --env-file .env.sample up

```
Админка доступна по адресу http://localhost:8001/admin

### Admin
Для запуска только админки нужно выполнить
```bash
docker compose up db nginx admin
```
Админка доступна по адресу http://localhost:8001/admin

### Etl
Для запуска только etl нужно выполнить
для скачивание elasticsearch потребуется vpn
```bash
docker compose up etl db elasticsearch
```
### Search
Для запуска только api поиска нужно выполнить
```bash
docker compose up search elasticsearch redis
```
API поиска доступна по адресу http://localhost:8000/api/openapi

### Тесты
Запустить все тесты поиска (вместе с окружением)
```
docker compose -f docker-compose-test.yml --env-file .env.test up
```
Если все ок, должно быть все зеленое и ...

### Локальный запуск
```
export PYTHONPATH=$PYTHONPATH:`pwd`
```