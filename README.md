# Momo Store: Docker Compose Portfolio Project

Учебный full-stack проект интернет-магазина, оформленный как DevOps-практика по контейнеризации backend/frontend приложения, локальному запуску через Docker Compose и базовой CI/security automation.

## Назначение

Проект демонстрирует:

- контейнеризацию Go backend и Vue frontend;
- многоэтапные Dockerfile;
- локальный запуск нескольких сервисов через Docker Compose;
- изолированные Docker-сети;
- healthcheck и endpoint для метрик;
- GitHub Actions workflows для deploy/security checks;
- базовую проверку контейнерных образов через Trivy workflow.

## Технологический стек

| Компонент | Технологии |
| --- | --- |
| Backend | Go |
| Frontend | Vue.js, Vue CLI |
| Reverse proxy / static | nginx-alpine |
| Infrastructure | Docker, Docker Compose |
| CI/CD | GitHub Actions |
| Security checks | Trivy workflow |

## Структура проекта

- `backend/` - Go backend, Dockerfile, `.dockerignore`, tests.
- `frontend/` - Vue frontend, Dockerfile, nginx config, `.dockerignore`.
- `docker-compose.yml` - локальный запуск backend/frontend и сетей.
- `.github/workflows/` - workflow для deploy и Trivy-проверок.

## Запуск проекта

### Сборка и запуск

```bash
docker compose build
docker compose up -d
```

### Просмотр логов

```bash
docker compose logs -f
```

### Проверка состояния

```bash
docker compose ps
```

### Остановка

```bash
docker compose down
```

## Доступ к сервисам

- Frontend: `http://localhost:80`
- Backend API: `http://localhost:8081`
- Backend metrics: `http://localhost:8081/metrics`
- Backend healthcheck: `http://localhost:8081/health`

## Реализованные DevOps-практики

### Backend image

- многоэтапная сборка;
- Alpine-based runtime image;
- статическая компиляция Go;
- запуск от непривилегированного пользователя;
- read-only filesystem;
- healthcheck.

### Frontend image

- двухэтапная сборка;
- nginx-alpine для раздачи статических файлов;
- отдельная nginx-конфигурация;
- оптимизированный `.dockerignore`.

### Docker Compose

- раздельные сервисы backend/frontend;
- изолированные Docker-сети;
- ограничения ресурсов CPU/memory;
- единая команда запуска локального стенда.

### CI / Security

- GitHub Actions workflow для deploy-процесса;
- отдельный Trivy workflow для security-check контейнерной части.

## Диагностика

```bash
# Использование ресурсов контейнерами
docker stats

# Логи конкретного сервиса
docker compose logs -f <service-name>

# Пересборка без cache
docker compose build --no-cache

# Очистка неиспользуемых Docker-ресурсов
docker system prune -f
```

## Практическая ценность

Проект релевантен для DevOps/Infrastructure портфолио, потому что показывает не только код приложения, но и эксплуатационный слой: Dockerfile, Compose, healthcheck, metrics endpoint, CI workflow и security scanning.

## Статус

Учебный проект. Для production-использования потребуются полноценная secret-management схема, registry, environments, rollback strategy и observability stack.
