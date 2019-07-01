FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
    libgmp-dev  \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    git \
    libzip-dev \
    zip \
    unzip \
    libxml2-dev \
    libgmp-dev re2c libmhash-dev libmcrypt-dev file \
    sudo \
    supervisor

RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include/

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-configure gmp \
  && docker-php-ext-install -j$(nproc) gd \
  && docker-php-ext-enable gd \
  && docker-php-ext-install pdo_mysql zip \
  && docker-php-ext-install gmp \
  && docker-php-ext-install exif \
  && docker-php-ext-install bcmath

RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo
USER docker

RUN mkdir -p /var/log/supervisor/

COPY supervisord.conf /etc/supervisor/supervisord.conf

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]

EXPOSE 9000