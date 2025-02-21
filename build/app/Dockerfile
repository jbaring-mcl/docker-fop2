FROM ubuntu:22.04

WORKDIR /usr/src

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=UTC

ENV MYSQL_USER \
    MYSQL_PASSWORD \
    MYSQL_DATABASE_FOP2 \
    MYSQL_HOST \
    MYSQL_PORT

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y make net-tools curl mysql-client procps msmtp gettext-base tzdata gnupg gosu curl ca-certificates zip unzip git supervisor sqlite3 libcap2-bin libpng-dev dnsutils librsvg2-bin fswatch \
    && apt-get install -y apache2 php php-cli php-common php-json php-pdo php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath php-sqlite3 php-dev libmcrypt-dev \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/*

ADD http://download2.fop2.com/fop2-2.31.39-debian-x86_64.tgz /usr/src/fop2.tgz

RUN tar xzf /usr/src/fop2.tgz \
    && rm -fr /usr/src/fop2.tgz \
    && cd /usr/src/fop2 && make install \
    && cp -fra /usr/src/fop2/server/fop2.cfg.docker /usr/local/fop2/fop2.cfg \
    && cp -fra /usr/src/fop2/server/fop2.docker.template.cfg /usr/local/fop2 \
    && cp -fra /usr/src/fop2/html/config.docker.php /var/www/html/fop2/config.php \
    && cp -fra /usr/src/fop2/html/admin/config.docker.php /var/www/html/fop2/admin/config.php \
    && cp -fra /usr/src/fop2/html/admin/functions-custom-dist.php /var/www/html/fop2/admin/functions-custom.php \
    && mkdir -p /usr/share/fop2 /var/lib/fop2 \
    && touch /usr/share/fop2/fop2.lic

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY start-apache2.sh /usr/local/bin/start-apache2.sh
COPY start-fop2.sh /usr/local/bin/start-fop2.sh
COPY start-container.sh /usr/local/bin/start-container.sh
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN chmod +x /usr/local/bin/start-apache2.sh
RUN chmod +x /usr/local/bin/start-fop2.sh
RUN chmod +x /usr/local/bin/start-container.sh

WORKDIR /usr/local/fop2

EXPOSE 80 4445

ENTRYPOINT ["start-container.sh"]
