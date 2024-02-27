FROM golang:1.21.6 AS base

EXPOSE 8080
WORKDIR /app

# Cache dependencies
COPY go.mod ./
# COPY go.sum ./

# Download dependencies. Dependencies will be cached if the go.mod and go.sum files are not changed
RUN go mod tidy

# Used for local development
FROM base as local

# Used as builder for deployment
FROM base as base-deploy

COPY . .
RUN go build -o myapp cmd/*

# RUN make build/api
RUN mkdir dist && go build -o dist ./...

# Used for deployment
FROM ubuntu:22.04 as deploy

RUN useradd --uid 10000 -m myuser
RUN apt-get update && apt-get install -y ca-certificates && update-ca-certificates

USER 10000

COPY --from=base-deploy --chown=10000 /app/myapp /usr/local/bin/myapp
CMD ["myapp"]
