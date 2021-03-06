FROM phusion/baseimage
MAINTAINER Fiona Hasanaj
ENV TZ=America/New_York

# install necessary software

RUN apt-get -y update && apt-get install -y \
    git \
    language-pack-en-base \
    libxslt1-dev \
    python-software-properties \
    vim \
    wget

RUN LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php5-5.6
RUN apt-get -y update && apt-get install -y \
    php5-dev \
    php5-cli \
    php5-curl \
    libapache2-mod-php5


RUN echo "memory_limit=-1" >> /etc/php5/cli/php.ini && \
    echo "date.timezone=America/New_York" >> /etc/php5/cli/php.ini

RUN wget -O /usr/local/bin/composer https://getcomposer.org/composer.phar && \
    chmod +x /usr/local/bin/composer

# copy over the necessary files
RUN mkdir /php-dev && mkdir /php-dev/tests
COPY composer.json /php-dev/composer.json
WORKDIR /php-dev

RUN composer install

# copy over the necessary files
COPY run_php.sh /php-dev/run_php.sh 
RUN chmod 755 /php-dev/run_php.sh

# specify docker volume
VOLUME ["/source"]

# allow interactive bash inside docker container
CMD ./run_php.sh $API_KEY $FILENAME $ALT_URL $GIT_USERNAME $VERSION
