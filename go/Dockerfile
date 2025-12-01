# Build stage
ARG GO_VERSION=1.23
FROM golang:${GO_VERSION}-alpine AS builder

WORKDIR /app

# Install dependencies for CGO (if needed)
# RUN apk add --no-cache gcc musl-dev

# Copy go mod files
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy source code
COPY . .

# Build the binary
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -o /app/main .

# Production stage - use scratch for smallest image
FROM scratch

# Copy binary from builder
COPY --from=builder /app/main /main

# Copy CA certificates for HTTPS (if needed)
# COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

EXPOSE 8080

ENTRYPOINT ["/main"]
