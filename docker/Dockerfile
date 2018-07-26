FROM php:7.2-apache
LABEL maintainer="ono.naoyaa@gmail.com"

# Install the required packages and remove the apt cache.
RUN apt-get update -yqq \
  && apt-get install -yqq --no-install-recommends \
    git \
    zip \
    unzip \
  && rm -rf /var/lib/apt/lists

# Enable PHP extensions
RUN docker-php-ext-install pdo_mysql mysqli

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

# Add cake command to system path
ENV PATH="${PATH}:/var/www/html/lib/Cake/Console"
ENV PATH="${PATH}:/var/www/html/app/Vendor/bin"

# COPY site conf file
COPY ./docker/apache/site.conf /etc/apache2/sites-available/000-default.conf

# Copy the code into /var/www/html/ inside the image
COPY . .

# Set default working directory
WORKDIR ./app

# Create tmp directory and make it writable by the web server
RUN mkdir -p \
    tmp/cache/models \
    tmp/cache/persistent \
  && chown -R :www-data \
    tmp \
  && chmod -R 770 \
    tmp

# Enable Apache modules and restart
RUN a2enmod rewrite \
  && service apache2 restart

EXPOSE 80
