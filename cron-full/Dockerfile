# Use debian for full cron support
ARG DEBIAN_VERSION=bookworm-slim
FROM debian:${DEBIAN_VERSION}

# Install cron and common utilities
RUN apt-get update && apt-get install -y --no-install-recommends \
    cron \
    ca-certificates \
    curl \
    tzdata \
    && rm -rf /var/lib/apt/lists/*

# Set timezone (optional)
ENV TZ=Asia/Shanghai

# Copy crontab files
# Method 1: User crontab
COPY crontab /etc/cron.d/app-cron

# Method 2: System cron directories (uncomment if needed)
# COPY cron.hourly/ /etc/cron.hourly/
# COPY cron.daily/ /etc/cron.daily/

# Set permissions (required for /etc/cron.d/)
RUN chmod 0644 /etc/cron.d/app-cron

# Apply cron job
RUN crontab /etc/cron.d/app-cron

# Copy scripts (if any)
# COPY scripts/ /scripts/
# RUN chmod +x /scripts/*.sh

# Run cron in foreground
CMD ["cron", "-f"]
