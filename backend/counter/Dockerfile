FROM golang:1.22-alpine AS builder

ENV CGO_ENABLED=0 \
    GOOS=linux


RUN apk add --no-cache build-base

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o go-backend -ldflags="-s -w" ./cmd/main.go

FROM alpine:3.20

WORKDIR /app

COPY --from=builder /app/go-backend .
COPY --from=builder /app/static ./static

RUN chmod +x /app/go-backend

EXPOSE 8080

CMD ["./go-backend"]
