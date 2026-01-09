FROM golang:1.24-alpine AS builder
WORKDIR /src/app
RUN apk add --no-cache ca-certificates
COPY go.mod . 
RUN go mod download
COPY . . 
RUN go build -o product-catalog ./app

FROM alpine:latest AS release

WORKDIR /src/app
COPY ./products ./products
COPY --from=builder /src/app/product-catalog ./
ENV PRODUCT_CATALOG_PORT=8080
ENTRYPOINT ["./product-catalog"]



