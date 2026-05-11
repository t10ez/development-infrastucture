# development-infrastructure

Local dev stack: message broker, databases, and observability. Use for local development only — no production hardening, no TLS, default credentials.

## Services

| Service  | Image                         | Ports                  | Purpose                                |
|----------|-------------------------------|------------------------|----------------------------------------|
| rabbitmq | `rabbitmq:3.13.7-management`  | 5672, 15672 (UI)       | AMQP broker + management UI            |
| mongo    | `mongo:7.0.3`                 | 27017                  | MongoDB, persisted to `./mongo/data/db`|
| redis    | `redis:7.4-alpine`            | 6379                   | Redis, persisted to `./redis/data`     |
| lgtm     | `grafana/otel-lgtm:latest`    | 3000 (Grafana), 4317, 4318 | OTel collector + Loki/Tempo/Mimir/Grafana all-in-one |

## Usage

```bash
cp .env.example .env   # optional — defaults work
docker compose up -d
docker compose down    # stop; data persists in ./mongo/data, ./redis/data
```

RabbitMQ UI: http://localhost:15672 (user/pass from `.env`, default `admin` / `admin`)
Grafana: http://localhost:3000

## Kind (Kubernetes)

`kind/kind.config` — single control-plane node with port 80 mapped to host and `ingress-ready=true` label.

```bash
kind create cluster --config kind/kind.config
```

## Notes

- Data directories (`mongo/data`, `redis/data`) are gitignored.
- `.env` is gitignored; commit `.env.example` only.
- Default credentials are for local dev — do not expose these ports publicly.
