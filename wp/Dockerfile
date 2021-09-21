  
FROM wordpress:5.8.1-php7.4-fpm


ENV PHPREDIS_VERSION=5.0.2 \
    CONFIG_VAR_FLAG=WPFPM_ \
    WP_PLUGINS="nginx-helper redis-cache" 

RUN apt-get update && apt-get -y install wget unzip

RUN docker-php-source extract \
  && curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
  && tar xfz /tmp/redis.tar.gz \
  && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
  && docker-php-ext-install redis \
  && docker-php-source delete \
  && curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
  && chmod +x wp-cli.phar \
  && mv wp-cli.phar /usr/local/bin/wp \
  && rm -r /tmp/*


COPY entrypoint.new.sh /usr/local/bin/
ENTRYPOINT [ "entrypoint.new.sh" ]
CMD ["php-fpm"]
