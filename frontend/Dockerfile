FROM node:21-alpine AS builder

ARG NODE_ENV=production
ENV VUE_APP_API_URL=/api
ENV NODE_OPTIONS=--openssl-legacy-provider

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Используем официальный unprivileged образ
FROM nginxinc/nginx-unprivileged:1.25-alpine

ARG NODE_ENV=production

# Копируем статику
COPY --from=builder /app/dist /usr/share/nginx/html

# Копируем кастомный конфиг
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Настройка healthcheck
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

