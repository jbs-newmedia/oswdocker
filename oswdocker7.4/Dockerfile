FROM debian:buster

LABEL maintainer="js@jbs-newmedia.de"
LABEL description="Debian / Apache / PHP"

#apache2
RUN apt update
RUN apt -y install apache2
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

#php7.4
RUN apt -y install wget lsb-release apt-transport-https ca-certificates
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
RUN apt update
RUN apt -y upgrade
RUN apt -y install php7.4
RUN apt -y install libapache2-mod-php7.4 php7.4-bcmath php7.4-gd php7.4-json php7.4-sqlite php7.4-mysqli php7.4-curl php7.4-xml php7.4-mbstring php7.4-zip mcrypt nano
RUN apt -y autoremove
RUN apache2ctl restart

#environment
RUN apt -y install locales
RUN locale-gen en_US.UTF-8
RUN locale-gen de_DE.UTF-8
RUN sed -i -e 's/^error_reporting\s*=.*/error_reporting = E_ALL/' /etc/php/7.4/apache2/php.ini
RUN sed -i -e 's/^display_errors\s*=.*/display_errors = On/' /etc/php/7.4/apache2/php.ini
RUN sed -i -e 's/^zlib.output_compression\s*=.*/zlib.output_compression = Off/' /etc/php/7.4/apache2/php.ini
RUN sed -i -e "s/memory_limit = .*/memory_limit = 512M/" /etc/php/7.4/apache2/php.ini
RUN sed -i -e "s/upload_max_filesize = .*/upload_max_filesize = 100M/" /etc/php/7.4/apache2/php.ini
RUN sed -i -e "s/post_max_size = .*/post_max_size = 100M/" /etc/php/7.4/apache2/php.ini
RUN sed -i -e "s/;date.timezone.*/date.timezone = Europe\/Berlin/" /etc/php/7.4/apache2/php.ini
RUN echo "opcache.revalidate_freq = 0" >> /etc/php/7.4/mods-available/opcache.ini
RUN a2enmod rewrite
RUN sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf
RUN chgrp -R www-data /var/www
RUN find /var/www -type d -exec chmod 775 {} +
RUN find /var/www -type f -exec chmod 664 {} +
EXPOSE 80
CMD ["/usr/sbin/apache2ctl","-DFOREGROUND"]
