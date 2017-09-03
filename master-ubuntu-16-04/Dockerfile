############################################################
# Dockerfile build ruby
# in ubuntu
############################################################

# Base image to ubuntu:16.04
FROM ubuntu:16.04

# Author
MAINTAINER Auli

RUN apt-get update -qq && \
    apt-get install -y sudo git vim bash-completion wget curl

# Chagne root password
RUN echo "root:docker" | chpasswd

# Setting timezone
ENV TZ 'Asia/Taipei'

# Gosu
ENV GOSU_VERSION 1.10
RUN set -ex; \
	\
	fetchDeps=' \
		ca-certificates \
		wget \
	'; \
	apt-get update; \
	apt-get install -y --no-install-recommends $fetchDeps; \
	rm -rf /var/lib/apt/lists/*; \
	\
	dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')"; \
	wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch"; \
	wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc"; \
	\
# verify the signature
	export GNUPGHOME="$(mktemp -d)"; \
	gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4; \
	gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu; \
	rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc; \
	\
	chmod +x /usr/local/bin/gosu && chmod +s /usr/local/bin/gosu && chown root:users /usr/local/bin/gosu; \
# verify that the binary works
	gosu nobody true; \
	\
	apt-get purge -y --auto-remove $fetchDeps

## Add user
RUN groupadd -r ubuntu && useradd -r -g ubuntu -m -s /bin/bash ubuntu
RUN echo "ubuntu:docker" | chpasswd && adduser ubuntu users && adduser ubuntu sudo
USER ubuntu
WORKDIR /home/ubuntu

CMD ["/bin/bash"]
