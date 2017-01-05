FROM ruby:2.2.3
MAINTAINER kisiel <kisielewicz777@gmail.com>

# Benefited from Paul Lam

# Install openjdk 8

RUN apt-get update && apt-get install -y --no-install-recommends \
		bzip2 \
		unzip \
		xz-utils \
	&& rm -rf /var/lib/apt/lists/*

RUN echo 'deb http://deb.debian.org/debian jessie-backports main' > /etc/apt/sources.list.d/jessie-backports.list
ENV LANG C.UTF-8
RUN { \
		echo '#!/bin/sh'; \
		echo 'set -e'; \
		echo; \
		echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
	} > /usr/local/bin/docker-java-home \
	&& chmod +x /usr/local/bin/docker-java-home

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

ENV JAVA_VERSION 8u111
ENV JAVA_DEBIAN_VERSION 8u111-b14-2~bpo8+1
ENV CA_CERTIFICATES_JAVA_VERSION 20140324

RUN set -x \
	&& apt-get update \
	&& apt-get install -y \
		openjdk-8-jdk="$JAVA_DEBIAN_VERSION" \
		ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION" \
	&& rm -rf /var/lib/apt/lists/* \
	&& [ "$JAVA_HOME" = "$(docker-java-home)" ]

RUN /var/lib/dpkg/info/ca-certificates-java.postinst configure


# Install Clojure/lein

ENV LEIN_VERSION=2.7.1
ENV LEIN_INSTALL=/usr/local/bin/

WORKDIR /tmp

RUN mkdir -p $LEIN_INSTALL \
  && wget --quiet https://github.com/technomancy/leiningen/archive/$LEIN_VERSION.tar.gz \
  && echo "Comparing archive checksum ..." \
  && echo "876221e884780c865c2ce5c9aa5675a7cae9f215 *$LEIN_VERSION.tar.gz" | sha1sum -c - \

  && mkdir ./leiningen \
  && tar -xzf $LEIN_VERSION.tar.gz  -C ./leiningen/ --strip-components=1 \
  && mv leiningen/bin/lein-pkg $LEIN_INSTALL/lein \
  && rm -rf $LEIN_VERSION.tar.gz ./leiningen \
  && chmod 0755 $LEIN_INSTALL/lein \
  && wget --quiet https://github.com/technomancy/leiningen/releases/download/$LEIN_VERSION/leiningen-$LEIN_VERSION-standalone.zip \
  && wget --quiet https://github.com/technomancy/leiningen/releases/download/$LEIN_VERSION/leiningen-$LEIN_VERSION-standalone.zip.asc \
  && gpg --keyserver pool.sks-keyservers.net --recv-key 2E708FB2FCECA07FF8184E275A92E04305696D78 \
  && echo "Verifying Jar file signature ..." \
  && gpg --verify leiningen-$LEIN_VERSION-standalone.zip.asc \
  && rm leiningen-$LEIN_VERSION-standalone.zip.asc \
  && mv leiningen-$LEIN_VERSION-standalone.zip /usr/share/java/leiningen-$LEIN_VERSION-standalone.jar \
  &&  apt-get update && apt-get install rlfe && rm -rf /var/lib/apt/lists/*

ENV PATH=$PATH:$LEIN_INSTALL
ENV LEIN_ROOT 1
