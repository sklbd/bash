FROM ubuntu:latest

LABEL author="Shakil Ahamed"
LABEL maintainer="sklahamed@gmail.com"

#====================================================================================
# Prevent user interaction in terminal.
#====================================================================================
ENV DEBIAN_FRONTEND noninteractive

#====================================================================================
# Update your OS
#====================================================================================
RUN apt-get update -y

#====================================================================================
# Install Apache2
#====================================================================================
RUN apt-get install apache2 -y

#====================================================================================
# Install PHP
#====================================================================================
RUN apt-get update && apt-get install -y \
    software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt-get update && apt-get install -y \
    php8.1 \
    libapache2-mod-php8.1 \
    php8.1-common \
    php8.1-mysql \
    php8.1-xml \
    php8.1-xmlrpc \
    php8.1-curl \
    php8.1-gd \
    php8.1-imagick \
    php8.1-cli \
    php8.1-imap \
    php8.1-mbstring \
    php8.1-opcache \
    php8.1-soap \
    php8.1-zip \
    php8.1-intl \
    php8.1-bcmath \
    unzip -y

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

#====================================================================================
# Add customized apache configuration to the container
#====================================================================================
ADD ./000-default.conf /etc/apache2/sites-available/

#====================================================================================
# Create directory for the project
# I follow a conventions where /home/<domain name>/<project name>
# Note: You can modify it to your own choice. I like it in this way.
#====================================================================================
RUN mkdir -vp /home/domain/demoProject

#====================================================================================
# Set a volume mount point within the container
VOLUME /home/domain/demoProject
#====================================================================================

#====================================================================================
# Add the PHP repository and install PHP Composer
#====================================================================================
RUN add-apt-repository ppa:ondrej/php && apt-get update && apt-get install -y composer

#====================================================================================
# Copy your code from host to the container
#====================================================================================
COPY ./code /home/domain/demoProject

#====================================================================================
# Set the current working directory into the container.
# Similar to command, cd /home/domain/demoProject
#====================================================================================
WORKDIR /home/domain/demoProject

#====================================================================================
# Install Laravel dependencies using Composer
#====================================================================================
RUN composer update

#====================================================================================
# Set permissions to allow Laravel to write to storage and bootstrap/cache directories
#====================================================================================
RUN chown -R www-data:www-data storage bootstrap/cache

#====================================================================================
# Enable Apache mod_rewrite for Laravel's routing
#====================================================================================
RUN a2enmod rewrite

#====================================================================================
# To solve an error
# AH00558: apache2: Could not reliably determine the server's fully qualified domain 
#====================================================================================
RUN   echo "ServerName 127.0.0.1" >> /etc/apache2/apache2.conf

#====================================================================================
# Expose port 80 for Apache
#====================================================================================
EXPOSE 80

#====================================================================================
# Start Apache web server
#====================================================================================
CMD ["apache2ctl", "-D", "FOREGROUND"]
