
FROM ubuntu
MAINTAINER kisiel <kisielewicz777@gmail.com>


# install wine and osslsigncode

RUN dpkg --add-architecture i386
RUN apt-get update -y
RUN apt-get install software-properties-common -y
RUN add-apt-repository ppa:ubuntu-wine/ppa
RUN apt-get update -y
RUN apt-get install wine1.8 -y winetricks xvfb
RUN apt-get install osslsigncode -y
RUN apt-get install -y openssl autoconf libcurl4-openssl-dev libssl-dev
#RUN apt-get install --install-recommends winehq-devel -y
RUN apt-get autoclean -y

# install java

RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer

  # Define commonly used JAVA_HOME variable
  ENV JAVA_HOME /usr/lib/jvm/java-8-oracle
