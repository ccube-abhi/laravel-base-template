FROM php:8.4-fpm

ARG WWWGROUP

# Copy composer.lock and composer.json
COPY composer.lock composer.json /var/www/html/

# Copy env example
COPY .env.example .env

# Set working directory
WORKDIR /var/www/html

# Set the timezone
ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install Additional System Dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    locales \
    libzip-dev \
    vim \
    unzip \
    git \
    curl \
    supervisor \
    libonig-dev \
    zip

    # Install required libs for GD
    RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libxpm-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) gd
# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN pecl install redis && docker-php-ext-enable redis
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl

# Install PHP Composer
#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
# Install Composer by copying it from the official Composer image
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Remove Cache
RUN rm -rf /var/cache/apk/*

# Copy existing application directory contents
COPY . /var/www/html

# Install project dependencies
ENV COMPOSER_PROCESS_TIMEOUT=2000
RUN composer install --no-interaction --prefer-dist

# clear route & config cache
RUN php artisan route:clear
RUN php artisan config:clear

# Add UID '1000' to www-data
RUN usermod -u 1000 www-data && \
    chown www-data /var/log/supervisor && \
    chown www-data /var/run

# Create required directories
RUN mkdir -p /var/www/html/storage/app \
    /var/www/html/storage/framework/cache \
    /var/www/html/storage/framework/sessions \
    /var/www/html/storage/framework/views \
    /var/www/html/storage/logs \
    /var/www/html/bootstrap/cache

# Set correct permissions (as root)
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache \
    && chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

# Copy source code with correct owner
COPY --chown=www-data:www-data . /var/www/html

# Switch user
USER www-data


# NOTE:
# - Laravel and Redis run inside Docker container
# - MySQL is hosted on your local machine (XAMPP/MAMP)
# - In .env, use: DB_HOST=host.docker.internal
# - If you run `php artisan serve` manually on your Mac, it uses host PHP/XAMPP, not Docker
