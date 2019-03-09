FROM drupal:8-apache

# Dependencies
RUN apt-get update && apt-get install -y \
		mysql-client \
    imagemagick \
	--no-install-recommends && rm -r /var/lib/apt/lists/*

	# install the PHP extensions we need
	RUN set -ex \
		&& buildDeps=' \
			libjpeg62-turbo-dev \
			libpng12-dev \
			libpq-dev \
	    libfreetype6-dev \
		' \
		&& apt-get update && apt-get install -y --no-install-recommends $buildDeps && rm -rf /var/lib/apt/lists/* \
		&& docker-php-ext-configure gd \
			--with-jpeg-dir=/usr \
			--with-png-dir=/usr \
	    --with-freetype-dir=/usr \
		&& docker-php-ext-install -j "$(nproc)" gd exif \
		&& apt-get purge -y --auto-remove $buildDeps

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

RUN a2enmod env headers
