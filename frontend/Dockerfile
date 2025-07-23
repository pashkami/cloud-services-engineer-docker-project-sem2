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

# Security: Set non-root user
RUN chown -R nginx:nginx /usr/share/nginx/html/momo-store
USER nginx

# Expose port and start Nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]