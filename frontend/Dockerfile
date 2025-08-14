FROM node:20-alpine AS builder

ENV VUE_APP_API_URL=/api
ENV NODE_OPTIONS=--openssl-legacy-provider

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci --no-audit --no-fund

COPY . .

RUN npm run build
RUN npm cache clean --force


# 5. Продакшн образ
FROM nginx:1-alpine-slim

COPY --from=builder /app/dist /usr/share/nginx/html/momo-store/
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

# Добавляем healthcheck
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -qO- http://localhost:80 || exit 1

CMD ["nginx", "-g", "daemon off;"]