# Usar a imagem base PHP 8.1 com Apache
FROM php:8.1-apache

# Definir a variável de ambiente para aceitar o EULA do Microsoft ODBC Driver
ENV ACCEPT_EULA=Y

# Atualizar pacotes e instalar ferramentas básicas e bibliotecas necessárias
RUN apt-get update && apt-get install -y \
    gnupg \
    curl \
    unixodbc-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd

# Habilitar o módulo mod_rewrite no Apache
RUN a2enmod rewrite

# Adicionar a chave do repositório Microsoft ODBC
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

# Adicionar o repositório Microsoft ODBC Driver
RUN curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

# Atualizar pacotes e instalar o Microsoft ODBC Driver
RUN apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql17

# Instalar extensões PHP para SQL Server
RUN pecl install sqlsrv pdo_sqlsrv \
    && docker-php-ext-enable sqlsrv pdo_sqlsrv

# Copiar o conteúdo da aplicação para o diretório do Apache
COPY . /var/www/html/

# Expor a porta 80 para acessar a aplicação
EXPOSE 80

# Comando para iniciar o Apache em primeiro plano
CMD ["apache2-foreground"]
