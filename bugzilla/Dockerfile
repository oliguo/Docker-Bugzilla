FROM ubuntu:18.04

EXPOSE 80/tcp
EXPOSE 22/tcp

ENV DB_HOST=bugzilla
ENV DB_NAME=bugzilla
ENV DB_USER=bugzilla
ENV DB_PASSWD=bugzilla

WORKDIR /app

VOLUME ["/var/www/html/bugzilla", "/var/log/apache2", "/home/bugzilla"]

RUN apt-get update \
&& apt-get -y install \
	mysql-client \
	apache2 \
	libappconfig-perl \
	libdate-calc-perl \
	libtemplate-perl \
	libmime-tools-perl \
	build-essential \
	libdatetime-timezone-perl \
	libemail-sender-perl \
	libemail-mime-perl \
	libdbi-perl \
	libdbd-mysql-perl \
	libcgi-pm-perl \
	libmath-random-isaac-perl \
	libapache2-mod-perl2 \
	libapache2-mod-perl2-dev \
	libchart-perl \
	libxml-perl \
	libxml-twig-perl \
	perlmagick \
	libgd-graph-perl \
	libsoap-lite-perl \
	libhtml-scrubber-perl \
	libjson-rpc-perl \
	libdaemon-generic-perl \
	libtheschwartz-perl \
	libtest-taint-perl \
	libauthen-radius-perl \
	libfile-slurp-perl \
	libencode-detect-perl \
	libmodule-build-perl \
	libnet-ldap-perl \
	libfile-mimeinfo-perl \
	libhtml-formattext-withlinks-perl \
	libfile-which-perl \
	libgd-dev \
	libmysqlclient-dev \
	graphviz \
	git \
	sudo \
	net-tools \
	iputils-ping \
	openssl \
	openssh-server \
	&& mkdir -p /var/run/sshd \
	&& apt-get -y clean

RUN echo 'root:123456' |chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN mkdir /root/.ssh

COPY bugzilla.conf startup.sh /app/
RUN git clone --branch release-5.0-stable https://github.com/bugzilla/bugzilla bugzilla
RUN cd bugzilla; /usr/bin/perl install-module.pl Email::Send

RUN useradd -c "bugzilla user" \
	--home-dir /home/bugzilla \
	--shell /bin/bash \
	--user-group \
	--groups sudo \
	bugzilla

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN chmod +x /app/startup.sh

ENTRYPOINT ["/app/startup.sh"]
