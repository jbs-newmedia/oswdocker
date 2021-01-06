FROM debian:buster

LABEL maintainer="js@jbs-newmedia.de"
LABEL description="Debian / Apache / PHP"

#apache2
RUN apt update
RUN apt -y install apache2
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

#php8.0
RUN apt -y install wget lsb-release apt-transport-https ca-certificates
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
RUN apt update
RUN apt -y upgrade
RUN apt -y install php8.0
RUN apt -y install libapache2-mod-php8.0 php8.0-bcmath php8.0-gd php8.0-sqlite php8.0-mysqli php8.0-curl php8.0-xml php8.0-mbstring php8.0-zip mcrypt nano
RUN apt -y autoremove
RUN sed -i "s|DocumentRoot /var/www/html|DocumentRoot /var/www/html\n\tPHPINIDir /var/www/php |g" /etc/apache2/sites-available/000-default.conf

#environment
RUN apt -y install locales
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen
RUN sed -i '/de_DE.UTF-8/s/^# //g' /etc/locale.gen
RUN locale-gen
RUN a2enmod rewrite
RUN sed -i '/<Directory \/var\/www\/>/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf
RUN chgrp -R www-data /var/www
RUN find /var/www -type d -exec chmod 775 {} +
RUN find /var/www -type f -exec chmod 664 {} +
RUN a2enmod rewrite
EXPOSE 80
CMD ["/usr/sbin/apache2ctl","-DFOREGROUND"]

#restart apache
RUN apache2ctl restart
