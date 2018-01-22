FROM debian:stretch
MAINTAINER Viktor Vorobjov

# Generate locale C.UTF-8 for postgres and general locale data

# make the "en_US.UTF-8" locale so postgres will be utf-8 enabled by default
# RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
#     && localedef -i en_GB -c -f UTF-8 -A /usr/share/locale/locale.alias en_GB.UTF-8
# ENV LANG en_GB.utf8
ENV LANG C.UTF-8

RUN apt-get update \
	&& apt-get install -y \
		tzdata \
	&& rm -rf /var/lib/apt/lists/*

RUN echo "Etc/UTC" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata


# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            ca-certificates \
            curl \
            node-less \
            python3-pip \
            python3-setuptools \
            python3-renderpm \
            libssl1.0-dev \
            xz-utils \
        && curl -o wkhtmltox.tar.xz -SL https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz \
        && echo '3f923f425d345940089e44c1466f6408b9619562 wkhtmltox.tar.xz' | sha1sum -c - \
        && tar xvf wkhtmltox.tar.xz \
        && cp wkhtmltox/lib/* /usr/local/lib/ \
        && cp wkhtmltox/bin/* /usr/local/bin/ \
        && cp -r wkhtmltox/share/man/man1 /usr/local/share/man/

# Tools
RUN apt-get update && apt-get install -y  \
        mc \
        git \
        git-core \
        wget \
        python3-dev \
        make \
        gcc \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge -y --auto-remove \
    && apt-get autoclean -y




#PostgreSQL Client

RUN set -ex; \
	if ! command -v gpg > /dev/null; then \
		apt-get update; \
		apt-get install -y --no-install-recommends \
			gnupg \
			dirmngr \
		; \
		rm -rf /var/lib/apt/lists/*; \
	fi

RUN set -ex; \
# pub   4096R/ACCC4CF8 2011-10-13 [expires: 2019-07-02]
#       Key fingerprint = B97B 0AFC AA1A 47F0 44F2  44A0 7FCC 7D46 ACCC 4CF8
# uid                  PostgreSQL Debian Repository
	key='B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8'; \
	export GNUPGHOME="$(mktemp -d)"; \
	gpg --keyserver p80.pool.sks-keyservers.net:80 --recv-keys "$key"; \
	gpg --export "$key" > /etc/apt/trusted.gpg.d/postgres.gpg; \
	rm -r "$GNUPGHOME"; \
	apt-key list

ENV PG_MAJOR 9.6
ENV PG_VERSION 9.6.6-1.pgdg80+1

RUN echo 'deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main' $PG_MAJOR > /etc/apt/sources.list.d/pgdg.list

RUN apt-get update \
	&& apt-get install -y \
		postgresql-client-$PG_MAJOR \
	&& rm -rf /var/lib/apt/lists/*

#PIP install
RUN pip3 install wheel
RUN pip3 install num2words  \
        pandas \
        psycogreen \
        gdata

RUN apt-get update && apt-get install -y  \
        libjpeg-dev \
        libjpeg62-turbo-dev \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge -y --auto-remove \
    && apt-get autoclean -y


RUN apt-get update && apt-get install -y  \
        libpq-dev \
        libxml2-dev \
        libxslt1-dev \
        libldap2-dev \
        libsasl2-dev \
        libffi-dev \
        libssl-dev \
        zlib1g-dev \
        build-essential \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge -y --auto-remove \
    && apt-get autoclean -y


RUN apt-get update && apt-get install -y  \
        software-properties-common \
        python3-software-properties \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge -y --auto-remove \
    && apt-get autoclean -y


RUN wget https://raw.githubusercontent.com/odoo/odoo/master/requirements.txt
RUN pip3 install -r requirements.txt
