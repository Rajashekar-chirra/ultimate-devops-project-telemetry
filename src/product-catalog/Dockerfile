# ============================================
# Product Catalog Service - Dockerfile
# Author: Shaan
# Language: Golang
# ============================================

# ----- STAGE 1: BUILD -----
FROM golang:1.22-alpine AS builder

# Add metadata
LABEL maintainer="shaan@devops"
LABEL service="product-catalog"

# Set working directory
WORKDIR /usr/src/app

# Copy dependency files first (for Docker layer caching)
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy source code
COPY . .

# Build the binary
# -o specifies output location
RUN go build -o /go/bin/product-catalog ./

# ----- STAGE 2: RUNTIME -----
FROM alpine:3.19 AS release

# Install ca-certificates (needed for HTTPS)
RUN apk add --no-cache ca-certificates

# Create non-root user (security best practice)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set working directory
WORKDIR /usr/src/app

# Copy products data file
COPY ./products ./products

# Copy binary from builder stage
COPY --from=builder /go/bin/product-catalog ./

# Change ownership to non-root user
RUN chown -R appuser:appgroup /usr/src/app

# Switch to non-root user
USER appuser

# Expose the port (from environment variable)
EXPOSE ${PRODUCT_CATALOG_PORT}

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:${PRODUCT_CATALOG_PORT}/_healthz || exit 1

# Run the application
ENTRYPOINT ["./product-catalog"]
