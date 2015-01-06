FROM base/archlinux
MAINTAINER David Parrish <daveparrish@gmail.com>

# Full system update.
RUN pacman -Syu --noconfirm --ignore filesystem \
    >/dev/null 2>/dev/null

RUN pacman -Syq --noconfirm nginx

ADD nginx.conf /etc/nginx/nginx.conf
ADD start_nginx.sh /start_nginx.sh
