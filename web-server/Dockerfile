FROM debian:wheezy

MAINTAINER webraketa <getsite@web-raketa.ru>

ENV DEBIAN_FRONTEND noninteractive

# Preparations for fish 2.0 installation
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-key D880C8E4 \
 && echo 'deb http://download.opensuse.org/repositories/shells:/fish:/release:/2/Debian_7.0/ ./' \
         > /etc/apt/sources.list.d/fish-shell.list

# fish shell requires TERM to be set
ENV TERM xterm

# Basic tools and GHC deps
RUN apt-get update \
  && apt-get -y upgrade \
  && apt-get install -y --no-install-recommends \
            locales \
            fish \
            wget \
            ca-certificates \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*
  
# Locale setup
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
 && locale-gen \
 && update-locale LANG=en_US.UTF-8 \
 && echo ": ${LANG=en_US.utf8}; export LANG" >> /etc/profile

RUN echo 'deb http://apt.postgresql.org/pub/repos/apt/ wheezy-pgdg main' \
         > /etc/apt/sources.list.d/pgdg.list

RUN wget --no-check-certificate --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

# Install tools, apache, PHP, and supplimentary programs. curl and lynx-cur are for debugging the container. --no-install-recommends    
RUN apt-get update \
 && apt-get install -y \
            git \
            bzip2 \
            make \
            autoconf \
            automake \
            sudo \
            mysql-server \
            mysql-client \
            apache2 \
            libapache2-mod-php5 \
            php5 \
            php5-mysql \
            php5-gd \
            php-pear \
            php-apc \
            php5-curl \
            postgresql-9.4 \
            curl \
            lynx-cur \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*
 
# Enable apache mods.
RUN a2enmod php5
RUN a2enmod rewrite

# Update the PHP.ini file, enable <? ?> tags and quieten logging.
RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php5/apache2/php.ini
RUN sed -i "s/error_reporting = .*$/error_reporting = E_ERROR | E_WARNING | E_PARSE/" /etc/php5/apache2/php.ini

# Manually set up the apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

EXPOSE 80

# inside index.php with phpinfo() function.
#ADD www /var/www/phpinfo

# Update the default apache site with the config we created.
#ADD apache-config.conf /etc/apache2/sites-enabled/000-default

# Stack support
# && passwd -d rocketman \
RUN mkdir /home/www \
 && useradd -m -d /home/www/rocketman -U -s /bin/bash rocketman -p "rocketman" \
 && echo "rocketman ALL=(ALL) NOPASSWD: ALL" \
    > /etc/sudoers.d/rocketman \
 && chmod 0440 /etc/sudoers.d/rocketman
ENV HOME /home/www/rocketman
WORKDIR /home/www/rocketman

USER rocketman

# Locale setup
RUN mkdir -p /home/www/rocketman/.config/fish \
 && echo "set -xg LANG en_US.utf8" \
         >> /home/www/rocketman/.config/fish/config.fish

USER root
RUN rm -fr /tmp/*

# By default, simply start apache.
CMD /usr/sbin/apache2ctl -D FOREGROUND