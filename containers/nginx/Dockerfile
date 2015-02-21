FROM base/archlinux
MAINTAINER David Parrish <daveparrish@gmail.com>

# Update mirrors and all packages
ADD mirrorlist /etc/pacman.d/mirrorlist.new
RUN rankmirrors -n 6 /etc/pacman.d/mirrorlist.new > /etc/pacman.d/mirrorlist \
&& pacman -Syuq --noconfirm && pacman-db-upgrade

RUN pacman -Syq --noconfirm nginx

ADD nginx.conf /etc/nginx/nginx.conf
ADD start_nginx.sh /start_nginx.sh
