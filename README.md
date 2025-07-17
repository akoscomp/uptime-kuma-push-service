# Uptime Kuma Push Service

[![GitHub go.mod Go version of a Go module](https://img.shields.io/github/go-mod/go-version/jjideenschmiede/uptime-kuma-push-service.svg)](https://golang.org/) [![Go](https://github.com/jjideenschmiede/uptime-kuma-push-service/actions/workflows/go.yml/badge.svg)](https://github.com/jjideenschmiede/uptime-kuma-server-push/actions/workflows/go.yml) [![Docker Image CI](https://github.com/jjideenschmiede/uptime-kuma-push-service/actions/workflows/docker-image.yml/badge.svg)](https://github.com/jjideenschmiede/uptime-kuma-server-push/actions/workflows/docker-image.yml) [![Docker Hub](https://img.shields.io/docker/pulls/jjdevelopment/uptime-kuma-push-service.svg)](https://hub.docker.com/r/jjdevelopment/uptime-kuma-push-service)

This Docker image is for sending a heartbeat to an [Uptime Kuma](https://github.com/louislam/uptime-kuma) server. Here you will find a little introduction on how to use it.

The application is written in Go (now using **Go 1.22**) and the Docker image is built using a multi-stage build with an Alpine Linux base for a minimal footprint.

## Environment Variables

| Variable | Default value |
|----------|:-------------:|
| URL      | default       |
| MSG      | OK            |
| CRON     | * * * * *     |

- **URL** must be set to your Uptime Kuma push endpoint.
- **MSG** is the message sent (default: OK).
- **CRON** is the cron schedule (default: every minute).

The milliseconds for the ping are calculated directly during the execution of the software.

## Health Check

A health check endpoint is available at [`/health`](http://localhost:8080/health) on port 8080. Example:

```console
curl http://localhost:8080/health
```

Response:
```json
{
  "status": "healthy",
  "timestamp": "2024-05-01T12:00:00Z",
  "service": "uptime-kuma-push"
}
```

## Launch Docker Container

To start the container properly, here is a small template. No volumes need to be mapped. The health check is available on port 8080.

```console
docker run -d --restart always \
  --name uptime-kuma-push-service \
  -e URL='https://uptime-kuma.test.de/api/push/M4KzP0tSTB' \
  -p 8080:8080 \
  jjdevelopment/uptime-kuma-push-service
```

## Docker Compose Example

You can also use Docker Compose. Here is an example `docker-compose.yaml`:

```yaml
services:
  uptime-kuma-push:
    image: jjdevelopment/uptime-kuma-push-service:latest
    restart: always
    environment:
      - URL=https://uptime-kuma.test.de/api/push/M4KzP0tSTB
      - MSG=OK
      - CRON=* * * * *
    ports:
      - "8080:8080"
```

## Build Locally (Optional)

If you want to run the docker image on a Raspberry Pi or other architecture, clone the repository and build the image:

```console
docker build -t jjdevelopment/uptime-kuma-push-service .
```

## Contribute

If you want to help with development, or have found a bug, open a [new issue](https://github.com/jjideenschmiede/uptime-kuma-push-service/issues).
