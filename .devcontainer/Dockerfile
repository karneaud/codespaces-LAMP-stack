# Use PHP 8.1 with Apache
FROM php:8.1-apache

# Install necessary packages and PHP extensions
RUN apt-get update && \
    apt-get install -y \
        libzip-dev \
        libpng-dev \
        libjpeg-dev \
        libfreetype6-dev \
        zlib1g-dev \
        unzip \
        git \
        mariadb-client && \
    docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && \
    docker-php-ext-install gd pdo_mysql zip mysqli && \
    a2enmod rewrite

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy the Apache configuration file from the host to the container
COPY apache-config.conf /etc/apache2/sites-available/000-default.conf

# Change the ownership of the public folder to www-data (Apache user)
RUN mkdir -p /var/www/html/public && chown -R www-data:www-data /var/www/html/public

# Enable the new configuration and restart Apache
RUN a2ensite 000-default && service apache2 restart

# Set working directory
WORKDIR /var/www/html
