# loop_infra

Единая инфраструктура для локального запуска backend-стека Loop.

## Что поднимается

- HTTP Gateway
- Auth Service
- User Service
- Analytics Service
- PostgreSQL для auth, user и analytics
- Redis для auth и user
- Kafka, Zookeeper, Kafka UI
- Kafka topics: `auth.events`, `user.events`

## Требования

Репозитории должны лежать рядом:

```text
Loop-company/
  loop_infra/
  http_gateway/
  auth_service/
  user_service/
  analytics-service/
```

## Запуск

```bash
docker compose up --build
```

Для старой версии Docker Compose:

```bash
docker-compose up --build
```

## Адреса

- HTTP Gateway: `http://localhost:8000`
- Auth gRPC: `localhost:50051`
- User gRPC: `localhost:50052`
- Analytics gRPC: `localhost:50053`
- Kafka external listener: `localhost:9093`
- Kafka UI: `http://localhost:8085`

## Data flow

```text
Client -> HTTP Gateway
          |-> Auth Service (gRPC) -> Kafka
          |-> User Service (gRPC) -> Kafka
          |-> Analytics Service (gRPC)

Auth/User events -> Kafka -> Analytics Service -> reports -> HTTP Gateway
```

HTTP Gateway не подключается к Kafka напрямую. Он общается с сервисами только по gRPC.
