FROM webraketa/web-server

MAINTAINER webraketa <getsite@web-raketa.ru>

#ARG SITENAME=local
#RUN SITENAME="gopa"
#RUN export SITENAME

RUN mkdir -p /var/www/sitename/public_html


# Add apache site with the config we created.
ADD apache-config.conf /etc/apache2/sites-enabled/sitename

EXPOSE 80

RUN service apache2 stop

CMD /usr/sbin/apache2ctl -D FOREGROUND
