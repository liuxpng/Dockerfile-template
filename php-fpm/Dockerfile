ARG PHP_VERSION=8.3

# ========== Build Stage ==========
FROM php:${PHP_VERSION}-fpm-alpine AS builder

# 安装构建工具
RUN apk add --no-cache --virtual .build-deps \
    autoconf \
    g++ \
    make

# Install common PHP extensions
RUN docker-php-ext-install \
    pdo_mysql \
    opcache

# Install Redis extension
RUN pecl install redis && docker-php-ext-enable redis

# Clean up build dependencies
RUN apk del .build-deps

# ========== Runtime Stage ==========
FROM php:${PHP_VERSION}-fpm-alpine AS runtime

# Install runtime dependencies
RUN apk add --no-cache \
    libpng \
    libjpeg-turbo \
    freetype \
    icu-libs \
    libzip

# Copy extensions from builder
COPY --from=builder /usr/local/lib/php/extensions/ /usr/local/lib/php/extensions/
COPY --from=builder /usr/local/etc/php/conf.d/ /usr/local/etc/php/conf.d/

# Create non-root user
RUN addgroup -g 1000 appgroup && \
    adduser -u 1000 -G appgroup -D appuser

WORKDIR /var/www/html

# Copy application code
COPY --chown=appuser:appgroup . .

# PHP-FPM configuration
RUN sed -i 's/user = www-data/user = appuser/' /usr/local/etc/php-fpm.d/www.conf && \
    sed -i 's/group = www-data/group = appgroup/' /usr/local/etc/php-fpm.d/www.conf

EXPOSE 9000

CMD ["php-fpm"]
