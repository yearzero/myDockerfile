FROM php:7.0-fpm

COPY ./sources.list.stretch /etc/apt/sources.list

RUN apt-get update \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libmagickwand-dev libmcrypt-dev  \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && :\
    && apt-get install -y libicu-dev libcurl4-openssl-dev \
    && docker-php-ext-install intl \
    && :\
    && apt-get install -y libxml2-dev \
    && apt-get install -y libxslt-dev \
    && docker-php-ext-install soap \
    && docker-php-ext-install xsl \
    && docker-php-ext-install xmlrpc \
    && docker-php-ext-install wddx \
    && :\
    && apt-get install -y libbz2-dev \
    && docker-php-ext-install bz2 \
    && :\
    && docker-php-ext-install zip \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install exif \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install calendar \
    && docker-php-ext-install sockets \
    && docker-php-ext-install gettext \
    && docker-php-ext-install shmop \
    && docker-php-ext-install sysvmsg \
    && docker-php-ext-install sysvsem \
    && docker-php-ext-install sysvshm \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install iconv \
    #&& docker-php-ext-install pdo_firebird \
    #&& docker-php-ext-install pdo_dblib \
    #&& docker-php-ext-install pdo_oci \
    #&& docker-php-ext-install pdo_odbc \
    #&& docker-php-ext-install pdo_pgsql \
    #&& docker-php-ext-install pgsql \
    #&& docker-php-ext-install oci8 \
    #&& docker-php-ext-install odbc \
    #&& docker-php-ext-install dba \
    #&& docker-php-ext-install interbase \
    #&& apt-get install -y curl \
    #&& apt-get install -y libcurl3 \
    #&& docker-php-ext-install curl \
    #&& :\
    #&& apt-get install -y libreadline-dev \
    #&& docker-php-ext-install readline \
    #&& :\
    #&& apt-get install -y libsnmp-dev \
    #&& apt-get install -y snmp \
    #&& docker-php-ext-install snmp \
    #&& :\
    #&& apt-get install -y libpspell-dev \
    #&& apt-get install -y aspell-en \
    #&& docker-php-ext-install pspell \
    #&& :\
    #&& apt-get install -y librecode0 \
    #&& apt-get install -y librecode-dev \
    #&& docker-php-ext-install recode \
    #&& :\
    #&& apt-get install -y libtidy-dev \
    #&& docker-php-ext-install tidy \
    #&& :\
    #&& apt-get install -y libgmp-dev \
    #&& ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/include/gmp.h \
    #&& docker-php-ext-install gmp \
    #&& :\
    #&& apt-get install -y postgresql-client \
    #&& apt-get install -y mysql-client \
    #&& :\
    #&& apt-get install -y libc-client-dev \
    #&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    #&& docker-php-ext-install imap \
    #&& :\
    #&& apt-get install -y libldb-dev \
    #&& apt-get install -y libldap2-dev \
    #&& docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu \
    && rm -rf /var/lib/apt/lists/*

# pecl 容易报错，自己下载安装，通过wget下载，统一在/usr/src这里下载
RUN set -xe; \
    fetchWget='wget'; \
    downDir='/usr/src'; \
    apt-get update; \
    apt-get install -y --no-install-recommends $fetchWget; \
    mkdir -p $downDir;

# protobuf
ENV PROTOBUF_DOWNLOADS_URL="https://github.com/allegro/php-protobuf/archive/master.zip"
ENV PROTOBUF_DOWNLOADS_NAME="protobuf.zip"
ENV PROTOBUF_DIR /usr/src/protobuf
RUN set -xe; \
    fetchDeps='unzip'; \
    apt-get update; \
    apt-get install -y --no-install-recommends $fetchDeps; \
    cd $downDir; \
    wget -O $PROTOBUF_DOWNLOADS_NAME "$PROTOBUF_DOWNLOADS_URL"; \
    unzip $PROTOBUF_DOWNLOADS_NAME -d $PROTOBUF_DIR; \
    rm $PROTOBUF_DOWNLOADS_NAME ; \
    cd $PROTOBUF_DIR/php-protobuf-master; \
    phpize; \
    ./configure; \
    make && make install; \
    rm -rf $PROTOBUF_DIR; \
    docker-php-ext-enable protobuf; \
    apt-get purge -y --auto-remove $fetchDeps;

# redis、xdebug、memcache
RUN # pecl install redis-3.1.4 \
    pecl install xdebug-2.6.0alpha1 \
    && docker-php-ext-enable xdebug \
    && :\
    && apt-get install -y libmagickwand-dev \
    && pecl install imagick-3.4.3 \
    && docker-php-ext-enable imagick \
    && :\
    && apt-get install -y libmemcached-dev zlib1g-dev \
    && pecl install memcached-3.0.4 \
    && docker-php-ext-enable memcached \
    && echo "memcached.use_sasl=1" >> /usr/local/etc/php/conf.d/docker-php-ext-memcached.ini

# rdkafka
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -yq \
        telnet \
        libsasl2-dev \
        libsasl2-modules-gssapi-mit \
        krb5-user \
        --no-install-recommends \
    && apt-get purge -y --auto-remove nginx \
    && docker-php-ext-install -j$(nproc) pcntl

ENV LIBRDKAFKA_DOWNLOADS_URL="https://github.com/edenhill/librdkafka/archive/v0.9.5.tar.gz"
ENV LIBRDKAFKA_DIR /usr/src/librdkafka
RUN set -xe; \
    apt-get update; \
    cd $downDir; \
    mkdir -p $LIBRDKAFKA_DIR; \
    wget -O librdkafka.tar.gz "$LIBRDKAFKA_DOWNLOADS_URL"; \
    tar -zxvf librdkafka.tar.gz -C $LIBRDKAFKA_DIR --strip-components=1; \
    rm librdkafka.tar.gz ; \
    apt-get purge -y --auto-remove $fetchDeps;

ENV RDKAFKA_DOWNLOADS_URL="http://pecl.php.net/get/rdkafka-3.0.3.tgz"
ENV RDKAFKA_DOWNLOADS_NAME="rdkafka-3.0.3"
RUN set -xe; \
    buildDeps='python'; \
    pecl uninstall rdkafka;\
    apt-get purge -y --auto-remove librdkafka-dev;\
    apt-get update; \
    apt-get install -y --no-install-recommends $buildDeps; \
    cd $LIBRDKAFKA_DIR;\
    ./configure; \
    sed -ri 's/^WITH_LDS.*/#WITH_LDS=y/g' $LIBRDKAFKA_DIR/Makefile.config; \
    make && make install; \
    \
    wget "$RDKAFKA_DOWNLOADS_URL"; \
    tar -xf $RDKAFKA_DOWNLOADS_NAME.tgz; \
    rm $RDKAFKA_DOWNLOADS_NAME.tgz; \
    cd $RDKAFKA_DOWNLOADS_NAME && phpize && ./configure && make && make install; \
    cd .. && rm -r $RDKAFKA_DOWNLOADS_NAME; \
    docker-php-ext-enable rdkafka; \
    apt-get purge -y --auto-remove $buildDeps; \
    rm -rf $LIBRDKAFKA_DIR;

# gearman
ENV GEARMAN_DOWNLOADS_NAME="gearman-2.0.5"
ENV GEARMAN_DOWNLOADS_URL="https://github.com/wcgallego/pecl-gearman/archive/$GEARMAN_DOWNLOADS_NAME.tar.gz"
RUN set -xe; \
    apt-get install -y libgearman-dev --no-install-recommends; \
    cd $downDir; \
    mkdir $GEARMAN_DOWNLOADS_NAME; \
    wget -O $GEARMAN_DOWNLOADS_NAME.tar.gz $GEARMAN_DOWNLOADS_URL; \
    tar zxvf $GEARMAN_DOWNLOADS_NAME.tar.gz -C $GEARMAN_DOWNLOADS_NAME --strip-components=1; \
    rm $GEARMAN_DOWNLOADS_NAME.tar.gz; \
    cd $GEARMAN_DOWNLOADS_NAME && phpize && ./configure && make && make install; \
    docker-php-ext-enable gearman; \
    cd .. && rm -r $GEARMAN_DOWNLOADS_NAME;

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer;

# CodeSniffer
RUN pear install PHP_CodeSniffer;

# phpunit
ENV PHPUNIT_DOWNLOADS_URL="https://phar.phpunit.de/phpunit-6.phar"
RUN set -xe; \
    cd $downDir; \
    wget -O phpunit.phar "$PHPUNIT_DOWNLOADS_URL"; \
    chmod +x phpunit.phar; \
    mv phpunit.phar /usr/local/bin/phpunit;

# ldap
RUN apt-get install -yq \
       libldb-dev \
       libldap2-dev \
       --no-install-recommends \
   && ln -s /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/libldap.so \
   && docker-php-ext-install -j$(nproc) ldap

RUN apt-get update \
    && apt-get install -y curl iputils-ping net-tools vim openbsd-inetd telnetd;

# 清空缓存和无用的软件
RUN set -xe; \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*; \
    apt-get purge -y --auto-remove $fetchWget;