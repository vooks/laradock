FROM vooksdev/laradock/php-fpm:latest

# Install composer
RUN curl -sS https://getcomposer.org/installer | php \
        && mv composer.phar /usr/local/bin/ \
        && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer

# Install NPM
RUN apt-get update && \
    apt-get install apt-transport-https && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get purge nodejs && \
    apt-get update && \
    apt-get install nodejs -y

COPY project/vooks-api/ /var/www/

RUN composer install --prefer-dist --no-dev
ENV PATH="~/.composer/vendor/bin:./vendor/bin:${PATH}"

RUN find /var/www/ -type d -exec chmod 755 {} \;
RUN find /var/www/ -type f -exec chmod 644 {} \;

RUN chown -R www-data: /var/www
RUN chmod -R 755 /var/www/storage

RUN rm .DS_Store

# Install npm dependencies and Build
RUN rm -rf node_modules
RUN npm install
RUN npm run dev

# Migrate
# CMD php artisan migrate --force

# Passport
RUN php artisan passport:keys

# Clear Caches 
# RUN php artisan optimize:clear

# Set working directory
WORKDIR /var/www
