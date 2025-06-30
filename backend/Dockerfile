
FROM golang:1.24.3-alpine AS former
WORKDIR /app
COPY . .
RUN apk add --no-cache git && \
  go mod download && \
  CGO_ENABLED=0 go build -o api ./cmd/api  

FROM alpine:3.22
WORKDIR /backend

# Копируем бинарник и необходимые файлы
COPY --from=former /app/api .

EXPOSE 8081

CMD ["./api"]