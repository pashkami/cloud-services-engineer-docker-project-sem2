
FROM golang:1.24.3-alpine AS build-stage
WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -o api ./cmd/api  


FROM alpine:3.22
WORKDIR /backend

RUN apk add --no-cache wget
RUN addgroup -g 1001 -S backend-group &&\
    adduser -u 1001 -S backender -G backend-group
USER backender

# Копируем бинарник и необходимые файлы
COPY --from=build-stage /app/api .

EXPOSE 8081

HEALTHCHECK \
  --interval=30s \
  --timeout=5s \
  --start-period=10s \
  --retries=5 \
  CMD wget -q --spider http://localhost:8081/health || exit 1

CMD ["./api"]