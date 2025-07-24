# syntax=docker/dockerfile:1

ARG NODE_VERSION=18.19.1
ARG NGINX_VERSION=1.25-alpine

################################################################################
# Stage 1: Build the application
################################################################################
FROM node:${NODE_VERSION}-alpine as build-stage

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci

#  OpenSSL flag test
ENV NODE_OPTIONS=--openssl-legacy-provider
# Copy source code and build
COPY . .
RUN npm run build

################################################################################
# Stage 2: Production with Nginx
################################################################################
FROM nginx:${NGINX_VERSION} as production-stage

# Remove default nginx configuration
RUN rm /etc/nginx/conf.d/default.conf

# Copy custom Nginx configuration
COPY nginx.conf /etc/nginx/conf.d

# Create directory for our app and copy build artifacts
RUN mkdir -p /usr/share/nginx/html/momo-store
COPY --from=build-stage /app/dist /usr/share/nginx/html/momo-store

# # Security: Set non-root user
# RUN chown -R nginx:nginx /usr/share/nginx/html/momo-store

RUN adduser -D -g '' appuser && \
    chown -R appuser:appuser /usr/share/nginx/html && \
    chown -R appuser:appuser /var/cache/nginx && \
    chown -R appuser:appuser /var/log/nginx && \
    chown -R appuser:appuser /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R appuser:appuser /var/run/nginx.pid

USER nginx

# Expose port and start Nginx
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]