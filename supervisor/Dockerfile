# Use alpine for minimal image
ARG ALPINE_VERSION=3.20
FROM alpine:${ALPINE_VERSION}

# Install supervisor and common utilities
RUN apk add --no-cache \
    supervisor \
    bash \
    tzdata

# Set timezone (optional)
ENV TZ=Asia/Shanghai

# Create directories
RUN mkdir -p /var/log/supervisor

# Copy supervisor configuration
COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY conf.d/ /etc/supervisor/conf.d/

# Expose any ports your services need
# EXPOSE 8080

CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf", "-n"]
