# Use the official PHP image with Apache
FROM php:8.1-apache

# Install necessary PHP extensions
RUN apt-get update && apt-get install -y \
    libicu-dev \
    libpq-dev \
    libzip-dev \
    zip \
    unzip \
    && docker-php-ext-install \
    intl \
    pdo \
    pdo_pgsql \
    pdo_mysql \
    zip \
    && a2enmod rewrite

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set the working directory
WORKDIR /data/www

# Copy the Symfony project files
COPY . .

# Set up permissions
RUN chown -R www-data:www-data /data/www/var /data/www/public

# Set environment variables
ENV APACHE_DOCUMENT_ROOT /data/www/public

# Expose the necessary port
EXPOSE 80

# Run Composer install
RUN composer install --no-scripts --no-autoloader

# Build the autoloader
RUN composer dump-autoload --optimize

# Clear the cache
RUN php bin/console cache:clear

# Start the Apache server
CMD ["apache2-foreground"]