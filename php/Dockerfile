# Base image
FROM php:8.2.18-apache

# Define environment variable
ENV INSTALL_DIR /var/www/html

# Change the line in apache2.conf to disable directory listing for security reasons
RUN sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf

# Install necessary packages
RUN apt-get update && apt-get install -y \
    zlib1g-dev libicu-dev g++ libzip-dev libpng-dev nano \
    libjpeg-dev libkrb5-dev zlib1g-dev libssl-dev libc-client-dev \
    libonig-dev libgmp-dev libmagickwand-dev libtidy-dev libxml2-dev \
    libxslt1-dev sqlite3 libsqlite3-dev

# Configure PHP extensions
RUN docker-php-ext-configure intl
RUN docker-php-ext-configure gd --with-jpeg
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl

# Install PHP extensions
RUN docker-php-ext-install -j$(nproc) \
    bz2 \
    calendar \
    exif \
    gd \
    gettext \
    gmp \
    imap \
    intl \
    mbstring \
    mysqli \
    opcache \
    pdo \
    pdo_mysql \
    pdo_sqlite \
    posix \
    soap \
    sockets \
    sysvmsg \
    sysvsem \
    sysvshm \
    tidy \
    xsl \
    zip

# Enable additional PHP extensions
RUN pecl install imagick igbinary \
    && docker-php-ext-enable imagick igbinary

# Enable Apache modules
RUN a2enmod rewrite expires

# Change shell for www-data user
RUN chsh -s /bin/bash www-data

# Set volume and working directory
VOLUME ["$INSTALL_DIR"]
WORKDIR $INSTALL_DIR
