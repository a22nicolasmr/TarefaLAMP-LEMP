FROM ubuntu:22.04

# Evitar prompts durante la instalación
ENV DEBIAN_FRONTEND=noninteractive

# Instalar software-properties-common para añadir repositorios
RUN apt-get update && apt-get install -y software-properties-common

# Añadir el repositorio de PHP y actualizar paquetes
RUN add-apt-repository ppa:ondrej/php -y && apt-get update

# Instalar Nginx, PHP 8.2 y sus módulos
RUN apt-get install -y \
    nginx \
    php8.2 \
    php8.2-fpm \
    php8.2-cli \
    php8.2-mysql \
    php8.2-common \
    php8.2-gd \
    php8.2-zip \
    php8.2-curl \
    php8.2-xml \
    php8.2-intl \
    php8.2-mbstring \
    php8.2-opcache \
    curl \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# Configurar permisos de la web
RUN chown -R www-data:www-data /var/www/html && chmod -R 755 /var/www/html

# Copiar configuración del virtualhost (descomentar si tienes el archivo)
# COPY ./nginx-lemp/vhost-nginx.conf /etc/nginx/conf.d/vhost-nginx.conf

# Instalar Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Descargar la última versión de PrestaShop
RUN curl -LO https://github.com/PrestaShop/PrestaShop/releases/download/8.2.1/prestashop_8.2.1.zip \
    && unzip prestashop_8.2.1.zip -d /var/www/html/ \
    && chown -R www-data:www-data /var/www/html/

# Exponer puertos
EXPOSE 9000 80 443

# Crear el directorio para el socket de PHP-FPM y asegurar permisos
RUN mkdir -p /run/php && chown -R www-data:www-data /run/php

# Modificar configuración de PHP-FPM para escuchar en un puerto en lugar de un socket
RUN sed -i 's|listen = /run/php/php8.2-fpm.sock|listen = 9000|' /etc/php/8.2/fpm/pool.d/www.conf

# Asegurar que PHP-FPM se inicie correctamente
CMD ["php-fpm8.2", "--nodaemonize"]
