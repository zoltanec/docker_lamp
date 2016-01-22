FROM webraketa/web-server

MAINTAINER webraketa <getsite@web-raketa.ru>

ARG SITENAME=local
RUN SITENAME="gopa"
RUN export SITENAME

RUN mkdir /home/www/rocketman/${SITENAME}
RUN mkdir /home/www/rocketman/${SITENAME}/public_html

# Add apache site with the config we created.
ADD apache-config.conf /etc/apache2/sites-enabled/www.${SITENAME}_vhost.conf

RUN service apache2 restart