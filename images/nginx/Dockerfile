FROM nginx:latest

RUN apt-get -y update --fix-missing
RUN apt-get upgrade -y
RUN apt-get install -y \
        apt-utils \
        nano \
        locate \
        curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libxslt-dev \
        libicu-dev \
        libmcrypt-dev \
        libxml2-dev


# VOLUME ["./images/nginx/hosts:/etc/nginx/conf.d", "./www:/var/www", "./logs:/var/log/nginx"]
EXPOSE 80 443

WORKDIR /var/www

CMD ["nginx", "-g", "daemon off;"]
