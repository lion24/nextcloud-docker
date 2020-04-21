FROM nextcloud:16.0-fpm-alpine

#Tweaks performance settings about fpm for nextcloud
COPY files/docker.conf /usr/local/etc/php-fpm.d/
COPY files/nextcloud.conf /usr/local/etc/php-fpm.d/
