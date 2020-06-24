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

# RUN npm run dev

# Set working directory
WORKDIR /var/www