	FROM dockerfile/php74-fpm:latest
	WORKDIR /application 

	# Fix debconf warnings upon build
	ARG DEBIAN_FRONTEND=noninteractive
	#ENV COMPOSER_ALLOW_SUPERUSER 1

	# Install selected extensions and other stuff
	RUN apt-get update \
    		&& apt-get -y --no-install-recommends install php7.4-memcached php7.4-mysql php7.4-gd php7.4-redis php7.4-imagick \
    		&& apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

	# Install git
	RUN apt-get update \
    		&& apt-get -y install git \
    		&& apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

	ARG uid=1000 
	RUN usermod -u $uid www-data 
	RUN apt-get update && \ 
    		apt-get upgrade -y 
	RUN apt-get install -y --force-yes curl git nano zlib1g-dev libcurl4-gnutls-dev \
    		&& docker-php-ext-install zip pdo pdo_mysql mbstring curl \
    		&& docker-php-ext-enable zip
	# Install composer
	ENV COMPOSER_ALLOW_SUPERUSER=1
	RUN curl -sS https://getcomposer.org/installer | php && \
		mv composer.phar /usr/local/bin/composer && \
	 	printf "\nPATH=\"~/.composer/vendor/bin:\$PATH\"\n" | tee -a ~/.bashrc 
	# composer plugin for parallel install
	RUN composer global require "hirak/prestissimo
