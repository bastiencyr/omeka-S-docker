FROM php:8.2-apache-bookworm

RUN a2enmod rewrite

RUN apt-get update && apt-get -y --no-install-recommends install \
    catdoc \
    docx2txt \
    imagemagick \
    # to generate pdf thumbnails
    ghostscript \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libjpeg-dev \
    libmemcached-dev \
    libmagickwand-dev \
    lynx \
    odt2txt \
    poppler-utils \
    tesseract-ocr \
    unzip \
    wget \
    zlib1g-dev

# Install the PHP extensions we need

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions; \
    install-php-extensions imagick pdo pdo_mysql xml solr

ARG OMEKA_VERSION=0.0
# Install Omeka S
RUN wget https://github.com/omeka/omeka-s/releases/download/v$OMEKA_VERSION/omeka-s-$OMEKA_VERSION.zip -O /var/www/omeka-s.zip \
    &&  unzip -q /var/www/omeka-s.zip -d /var/www/ \
    &&  rm /var/www/omeka-s.zip \
    &&  rm -rf /var/www/html/ \
    &&  mv /var/www/omeka-s/ /var/www/html/

# Install Common module
RUN wget https://github.com/Daniel-KM/Omeka-S-module-Common/releases/download/3.4.56/Common-3.4.56.zip -O /var/www/html/modules/Common.zip \
    && unzip -q /var/www/html/modules/Common.zip -d /var/www/html/modules \
    && rm /var/www/html/modules/Common.zip

# Install EasyAdmin
RUN wget https://github.com/Daniel-KM/Omeka-S-module-EasyAdmin/releases/download/3.4.17/EasyAdmin-3.4.17.zip -O /var/www/html/modules/EasyAdmin.zip \
    && unzip -q /var/www/html/modules/EasyAdmin.zip -d /var/www/html/modules \
    && rm /var/www/html/modules/EasyAdmin.zip

# Copy database configuration
COPY ./database.ini /var/www/html/config/
COPY ./php.ini /usr/local/etc/php/conf.d/

RUN chown -R www-data:www-data /var/www/html/ \
   && chmod 600 /var/www/html/config/database.ini

CMD ["apache2-foreground"]