FROM php:5.6-fpm

RUN apt-get update && apt-get install -y libmcrypt-dev
RUN	apt-get install -y curl wget telnet
RUN	apt-get install -y libfreetype6-dev
RUN	apt-get install -y libjpeg62-turbo-dev
RUN	apt-get install -y libmcrypt-dev
RUN	apt-get install -y xml-core
RUN	apt-get install -y libpng12-dev
RUN	apt-get install -y libxml2-dev
RUN	apt-get install -y php-http-request
RUN	apt-get install -y mysql-client
	

# Install php mysql extension	
RUN docker-php-ext-install pdo_mysql

# Install php mcrypt
RUN	docker-php-ext-install -j$(nproc) mcrypt
RUN	docker-php-ext-enable mcrypt

# Install php soap
RUN	docker-php-ext-install -j$(nproc) soap
RUN	docker-php-ext-enable soap

# Install php gd
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

# Install ioncube
WORKDIR /tmp
RUN wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz \
	&& tar -zxvf ioncube_loaders_lin_x86-64.tar.gz \
	&& cp ioncube/ioncube_loader_lin_5.6.so $(dirname $(find /usr/local/lib/php/extensions/ -name soap.so)) \
	&& rm -rf ioncube_loaders_lin_x86-64.tar.gz ioncube \
	&& echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name ioncube_loader_lin_5.6.so)" > /usr/local/etc/php/conf.d/01.ioncube.ini

# Install xdebug && create xdebug.ini
RUN pecl install xdebug
RUN echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini
RUN echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/xdebug.ini
RUN echo "xdebug.remote_port=9050" >> /usr/local/etc/php/conf.d/xdebug.ini
RUN echo "xdebug.idekey=PHPSTORM_KEY" >> /usr/local/etc/php/conf.d/xdebug.ini

# Add n98-magerun to the container
RUN wget https://files.magerun.net/n98-magerun.phar \
	&& chmod +x n98-magerun.phar \
	&& mv n98-magerun.phar /usr/local/bin/n98-magerun

# Create some aliases
RUN echo 'alias xdebug_off="mv /usr/local/etc/php/conf.d/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini.off"' >> ~/.bashrc
RUN echo 'alias xdebug_on="mv /usr/local/etc/php/conf.d/xdebug.ini.off /usr/local/etc/php/conf.d/xdebug.ini"' >> ~/.bashrc