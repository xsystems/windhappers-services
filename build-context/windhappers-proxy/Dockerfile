FROM httpd:2.4

MAINTAINER xsystems

RUN echo deb http://ftp.debian.org/debian jessie-backports main >> /etc/apt/sources.list

RUN apt-get update -qq && \
    apt-get install -qq -t jessie-backports -y python-certbot-apache cron

RUN apt-get -qq clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN a2enmod headers proxy_http ssl

COPY ./httpd-windhappers-proxy.conf /etc/apache2/sites-available/

COPY letsencrypt.ini /etc/letsencrypt/cli.ini
COPY windhappers-proxy /usr/local/bin/

RUN echo "0 7,19 * * * date --iso-8601=minutes >> /var/log/certbot.log && certbot renew >> /var/log/certbot.log" | crontab -

EXPOSE 80 443

CMD ["windhappers-proxy"]
# CMD bash
