# Build stage
ARG RUST_VERSION=1.83
FROM rust:${RUST_VERSION}-alpine AS builder

WORKDIR /app

# Install musl-dev for static linking
RUN apk add --no-cache musl-dev

# Copy manifests
COPY Cargo.toml Cargo.lock ./

# Create dummy src to cache dependencies
RUN mkdir src && echo "fn main() {}" > src/main.rs
RUN cargo build --release
RUN rm -rf src

# Copy actual source code
COPY src ./src

# Build the binary (touch to update mtime)
RUN touch src/main.rs && cargo build --release

# Production stage - use scratch for smallest image
FROM scratch

# Copy binary from builder
COPY --from=builder /app/target/release/app /app

# Copy CA certificates for HTTPS (if needed)
# COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

EXPOSE 8080

ENTRYPOINT ["/app"]
