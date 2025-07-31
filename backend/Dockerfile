# syntax=docker/dockerfile:1

ARG GO_VERSION=1.22.7-alpine

# Build stage
FROM golang:${GO_VERSION} AS build
WORKDIR /src

# Copy dependencies first
COPY go.mod go.sum ./
RUN go mod download -x

# Copy source code
COPY . .

# Build arguments
ARG TARGETARCH="amd64"
ARG TARGETOS="linux"

# Build application
RUN CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} \
    go build -ldflags="-s -w" -o /app/server ./cmd/api

# Runtime stage
FROM alpine:3.21

# Install runtime dependencies
RUN apk add --no-cache ca-certificates tzdata

# Create non-root user
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser

# Set secure working directory
WORKDIR /app
RUN chown appuser:appuser /app

# Copy binary with proper permissions
COPY --from=build --chown=appuser:appuser /app/server /app/server

# Runtime configuration
USER appuser
EXPOSE 8081
ENTRYPOINT ["/app/server"]