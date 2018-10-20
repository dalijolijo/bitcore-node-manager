# 2018 The Bitcore BTX Core Developers (dalijolijo)
# BitCore Node Manager Docker Solution - Dockerfile

# Use an official Ubuntu runtime as a parent image
FROM ubuntu:16.04

LABEL maintainer="Jon D. (dArkjON), David B. (dalijolijo)"
LABEL bitcore_version="0.15.2.0.0"

# Make ports available to the world outside this container
# DefaultPort = 8555
# RPCPort = 8556
# TorPort = 9051
# BNM = 8080
USER root

# Change sh to bash
SHELL ["/bin/bash", "-c"]

RUN echo '*** BitCore Node Manager Docker Solution ***'

#
# Running updates and installing required packages
#
# Installing required packages for bitcore compiling
RUN echo '*** Running updates and installing required packages ***' && \
    apt-get update -y && \
    apt-get install -y  apt-utils \
                        autoconf \
                        automake \
                        autotools-dev \
                        build-essential \
			curl \
                        git \
                        libboost-all-dev \
                        libevent-dev \
                        libminiupnpc-dev \
                        libssl-dev \
                        libtool \
                        pkg-config \
                        software-properties-common \
			sudo \
			supervisor \
                        vim \
			wget
RUN sudo add-apt-repository ppa:bitcoin/bitcoin
RUN sudo apt-get update && \
    sudo apt-get -y upgrade
RUN apt-get install -y libdb4.8-dev \
                       libdb4.8++-dev

#
# Installing required packages for bitcore-node-manager
#
RUN echo '*** Running updates and installing required packages for bitcore-node-manager***' && \
    apt-get update -y && \
    apt-get install -y 	apache2 \
		       	php \
 	       		libapache2-mod-php php7.0-curl \
		       	screen

#
# Install bitcore-node-manager
#
RUN git clone https://github.com/dalijolijo/bitcore-node-manager.git /var/www/html/btx

#
# Download BitCore release
#
RUN echo '*** Download BitCore release ***' && \
    mkdir -p /root/src && \
    cd /root/src && \
    wget https://github.com/LIMXTEC/BitCore/releases/download/0.15.2.0.0/linux.Ubuntu.16.04.LTS-static-libstdc.tar.gz && \
    tar xzf *.tar.gz && \
    chmod 775 bitcore* && \
    cp bitcore* /usr/local/bin && \
    rm *.tar.gz

#
# Configure bitcore.conf	
#	
COPY bitcore.conf /root/src	
RUN echo '*** Configure bitcore.conf ***' && \	
    mkdir -p /root/.bitcore && \	
    cp /root/src/bitcore.conf /root/.bitcore/bitcore.conf

#
# Copy Supervisor Configuration
#
COPY *.sv.conf /etc/supervisor/conf.d/

#
# Copy start script
#
RUN echo '*** Copy start script ***'
COPY start.sh /usr/local/bin/start.sh
RUN rm -f /var/log/access.log && mkfifo -m 0666 /var/log/access.log && \
    chmod 755 /usr/local/bin/*

ENV TERM linux
CMD ["/usr/local/bin/start.sh"]
