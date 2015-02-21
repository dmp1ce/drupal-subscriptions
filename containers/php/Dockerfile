FROM base/archlinux
MAINTAINER David Parrish <daveparrish@gmail.com>

# Update mirrors and all packages
ADD mirrorlist /etc/pacman.d/mirrorlist.new
RUN rankmirrors -n 6 /etc/pacman.d/mirrorlist.new > /etc/pacman.d/mirrorlist \
&& pacman -Syuq --noconfirm && pacman-db-upgrade

# Install PHP
RUN pacman -Syq --noconfirm --needed php-fpm php-sqlite php-gd \
>/dev/null 2>/dev/null

# Configure PHP
RUN sed -i 's/;extension=pdo_mysql.so/extension=pdo_mysql.so/g' /etc/php/php.ini
RUN sed -i 's/;extension=pdo_sqlite.so/extension=pdo_sqlite.so/g' /etc/php/php.ini
RUN sed -i 's/;extension=gd.so/extension=gd.so/g' /etc/php/php.ini
RUN sed -i 's/memory_limit = 128M/memory_limit = 256M/g' /etc/php/php.ini
RUN sed -i 's/;mbstring.http_input =/mbstring.http_input = pass/g' /etc/php/php.ini
RUN sed -i 's/;mbstring.http_output =/mbstring.http_output = pass/g' /etc/php/php.ini

# Configure PHP-FPM listening port
RUN sed -i '/^listen/c \
listen = 0.0.0.0:9000' /etc/php/php-fpm.conf

# Install drush (and drush dependencies)
# mariadb required for "drush sqlc"
RUN pacman -Syq --needed --noconfirm base-devel git jshon mariadb \
    >/dev/null 2>/dev/null
# Get AUR helper to install drush
RUN git clone https://github.com/oshazard/apacman /opt/apacman \
&& chmod +x /opt/apacman/apacman \
&& /opt/apacman/apacman -S --noedit --noconfirm drush
# Set HOME and open_basedir so that drush doesn't complain
ENV HOME /root
RUN mkdir /root/.drush
RUN echo "open_basedir =" >> /root/.drush/drush.ini

# Turn on mysql client auto-complete
RUN sed -i 's/no-auto-rehash/auto-rehash/g' /etc/mysql/my.cnf

# Allow nginx to find service
EXPOSE 9000

# Add script for running service
ADD start_php_fpm.sh /opt/start_php_fpm.sh

## Install msmtp and configure to connect to postfix service
#RUN pacman -Syq --needed --noconfirm msmtp \
#    >/dev/null 2>/dev/null
#ADD msmtp.conf /etc/msmtp.conf
## Configure php sendmail to use the msmtp program
#RUN sed -i 's);sendmail_path =)sendmail_path = "/usr/sbin/msmtp -C /etc/msmtp.conf -a default -t")g' /etc/php/php.ini
## Important for allowing FPM to access config since FPM runs as http
#RUN chmod 600 /etc/msmtp.conf
#RUN chown http:http /etc/msmtp.conf

# Move root directory
#RUN sed -i '/^;chdir/c \
#chdir = /code/drupal' /etc/php/php-fpm.conf

# Place the log location
#RUN sed -i '/^;error_log/c \
#error_log = /log/php-fpm.log' /etc/php/php-fpm.conf
#RUN sed -i '/^;access.log/c \
#access.log = /log/$pool.access.log' /etc/php/php-fpm.conf
#RUN sed -i '/^;slowlog/c \
#slowlog = log/$pool.log.slow' /etc/php/php-fpm.conf

# vi: set tabstop=4 expandtab :
