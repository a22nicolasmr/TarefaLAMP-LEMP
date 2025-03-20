FROM ubuntu:22.04

# Evitar prompts durante la instalación
ENV DEBIAN_FRONTEND=noninteractive

# Instalar dependencias básicas
RUN apt-get update && apt-get install -y \
    apache2 \
    php \
    php-cli \
    php-mysql \
    php-gd \
    php-zip \
    php-curl \
    php-xml \
    php-intl \
    php-mbstring \
    libapache2-mod-php \
    curl \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# Configurar permisos
RUN chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html

# Habilitar módulos de Apache
RUN a2enmod rewrite
RUN a2enmod ssl
# RUN a2ensite default-ssl

# Copiar configuración del virtualhost (nombre corregido)
COPY vhost-apache.conf /etc/apache2/sites-available/000-default.conf

# Instalar Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Descargar PrestaShop
# Dockerfile (corregido)
RUN curl -LO https://github.com/PrestaShop/PrestaShop/releases/download/8.1.5/prestashop_8.1.5.zip \
    && unzip prestashop_8.1.5.zip -d /var/www/html/ \
    && chown -R www-data:www-data /var/www/html/

# Puerto y comando de inicio
EXPOSE 80 443
CMD ["apache2ctl", "-D", "FOREGROUND"]