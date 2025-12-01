# Use alpine for minimal image
ARG ALPINE_VERSION=3.20
FROM alpine:${ALPINE_VERSION}

# Install necessary packages
RUN apk add --no-cache \
    dcron \
    bash \
    tzdata

# Set timezone (optional)
ENV TZ=Asia/Shanghai

# Copy crontab file
COPY crontab /etc/crontabs/root

# Ensure correct permissions
RUN chmod 0644 /etc/crontabs/root

# Copy scripts (if any)
# COPY scripts/ /scripts/
# RUN chmod +x /scripts/*.sh

# Run crond in foreground
CMD ["crond", "-f", "-l", "2"]
