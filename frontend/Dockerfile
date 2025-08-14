# Используем официальный образ Node.js на базе Alpine Linux как этап сборки
FROM node:20-alpine AS builder

# Устанавливаем переменные окружения для сборки Vue-приложения
ENV VUE_APP_API_URL=/api
ENV NODE_OPTIONS=--openssl-legacy-provider

# Создаем рабочую директорию внутри контейнера
WORKDIR /app

# Копируем только файлы зависимостей (оптимизация кэширования слоев)
COPY package.json package-lock.json ./

# Устанавливаем зависимости
RUN npm ci --no-audit --no-fund

COPY . .

# Собираем production-версию фронтенда
RUN npm run build

# Очищаем кэш npm для уменьшения размера образа
RUN npm cache clean --force


# Cоздаем оптимизированный production-образ
FROM nginx:1-alpine-slim

# Копируем собранные статические файлы из этапа builder
COPY --from=builder /app/dist /usr/share/nginx/html/momo-store/


# Копируем конфигурационный файл nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

# Запускаем nginx
CMD ["nginx", "-g", "daemon off;"]