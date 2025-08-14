# syntax=docker/dockerfile:1


# Определяем аргумент для версии Go с Alpine Linux
ARG GO_VERSION=1.22.7-alpine

# Этап сборки приложения
FROM golang:${GO_VERSION} AS build
WORKDIR /src

# Копируем файлы зависимостей в первую очередь (оптимизация кэширования)
COPY go.mod go.sum ./

# Загружаем зависимости с выводом деталей процесса (-x)
RUN go mod download -x

# Копируем весь исходный код приложения
COPY . .


ARG TARGETARCH="amd64"
ARG TARGETOS="linux"

# Компилируем приложение
RUN CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} \
    go build -ldflags="-s -w" -o /app/server ./cmd/api

# Финальный этап: создаем production-образ
FROM alpine:3.21

# Устанавливаем необходимые для работы пакеты
RUN apk add --no-cache ca-certificates tzdata

# Создаем непривилегированного пользователя для безопасности
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser

# Создаем рабочую директорию и устанавливаем владельца
WORKDIR /app
RUN chown appuser:appuser /app

# Копируем собранный бинарник из этапа сборки
COPY --from=build --chown=appuser:appuser /app/server /app/server


# Переключаемся на непривилегированного пользователя
USER appuser
EXPOSE 8081

ENTRYPOINT ["/app/server"]