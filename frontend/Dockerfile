FROM node:21-bullseye AS builder

# ARG NODE_ENV=production
# ENV VUE_APP_API_URL=/api
# ENV NODE_OPTIONS=--openssl-legacy-provider

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

# 5. Продакшн образ
FROM nginx:1.25-alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
