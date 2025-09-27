# 1. Builder
FROM golang:1.25.1-alpine AS builder
WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 go build -o backend ./cmd/api/


# 2. Runner
FROM alpine:3.22.1
WORKDIR /app

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=builder /app/backend .

RUN chown appuser:appgroup /app/backend

RUN apk add --no-cache curl

USER appuser

EXPOSE 8081
CMD ["./backend"]