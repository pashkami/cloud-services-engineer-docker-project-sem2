#Стадия сборки
FROM node:18-alpine AS builder

WORKDIR /app

COPY package.json ./
RUN npm ci

# Копируем исходный код и собираем проект
COPY . .
RUN npm run build


FROM nginx:1.25-alpine

# Удаляем дефолтную конфигурацию Nginx
RUN rm /etc/nginx/conf.d/default.conf

# Копируем нашу конфигурацию
COPY nginx.conf /etc/nginx/conf.d


COPY --from=builder /app/dist /usr/share/nginx/html

# Открываем порт и запускаем Nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]