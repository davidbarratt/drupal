FROM drupal:9

# Dependencies
RUN apt-get update && apt-get install -y \
		unzip \
    	imagemagick \
	--no-install-recommends && rm -r /var/lib/apt/lists/*

# Extensions
RUN pecl install \
		APCu-5.1.19 \
		# uploadprogress-1.1.3 \
	; \
	docker-php-ext-enable \
		apcu \
		# uploadprogress \
	;

# Set the max upload size.
RUN { \
		echo 'upload_max_filesize = 32M'; \
		echo 'post_max_size = 32M'; \
	} > /usr/local/etc/php/conf.d/upload-filesize.ini

# Optomize container for a low-memory VPS.
RUN { \
		echo 'StartServers 1'; \
		echo 'MinSpareServers 1'; \
	} | tee "$APACHE_CONFDIR/conf-available/low-memory.conf" \
	&& a2enconf low-memory

# Enable Apache modules
RUN a2enmod headers
