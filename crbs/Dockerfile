FROM docker.io/library/php:7.3-fpm-alpine

#Change version to trigger build
ARG CRBS_VERSION=2.8.6

WORKDIR /var/www/html

# Install dependencies
RUN apk update && apk add --no-cache \
    mysql-client \
    openldap-dev\
    freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev oniguruma-dev \
    icu-libs \
    jpegoptim optipng pngquant gifsicle \
    supervisor \
    apache2 \
    apache2-ctl \
    apache2-proxy

# Installing extensions
RUN docker-php-ext-install mysqli pdo_mysql mbstring exif pcntl pdo bcmath opcache ldap

#RUN docker-php-ext-configure gd --enable-gd --with-jpeg=/usr/include/ --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# Downloading and installing Classroombookings
RUN curl -LJO https://api.github.com/repos/classroombookings/classroombookings/zipball/v${CRBS_VERSION} && \
    mv classroombookings-classroombookings-v${CRBS_VERSION}* crbs-${CRBS_VERSION}.zip && \
    unzip crbs-${CRBS_VERSION}.zip && \
    cp -r classroombookings*/. ./ && \
    rm -r classroombookings* && \
    rm crbs-${CRBS_VERSION}.zip

RUN chown www-data -R .

COPY ./start.sh /start.sh
RUN chmod +x /start.sh

COPY config/custom.ini /usr/local/etc/php/conf.d/custom.ini

# Configure supervisord
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY config/app.conf  /etc/apache2/conf.d/app.conf

RUN sed -i '/LoadModule rewrite_module/s/^#//g' /etc/apache2/httpd.conf && \
    sed -i 's#AllowOverride [Nn]one#AllowOverride All#' /etc/apache2/httpd.conf && \
    sed -i '$iLoadModule proxy_module modules/mod_proxy.so' /etc/apache2/httpd.conf

# RUN mkdir -p "/sessions" && chown www-data:www-data /sessions && chmod 0777 /sessions
# VOLUME [ "/sessions" ]

# Expose port 9000 and start php-fpm server
ENTRYPOINT ["/start.sh"]
EXPOSE 80

