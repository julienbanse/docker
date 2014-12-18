FROM ubuntu:14.04
MAINTAINER Julien Banse, "julien.banse@gmail.com"

RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu precise main" | tee /etc/apt/sources.list.d/webupd8team-java.list
RUN echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu precise main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886

RUN apt-get update && apt-get clean
RUN apt-get install -q -y wget
RUN apt-get install -q -y zip

# Never ask for confirmations
ENV DEBIAN_FRONTEND noninteractive
RUN echo "debconf shared/accepted-oracle-license-v1-1 select true" | /usr/bin/debconf-set-selections
RUN echo "debconf shared/accepted-oracle-license-v1-1 seen true" | /usr/bin/debconf-set-selections

# Add oracle-jdk7 to repositories
RUN apt-get install python-software-properties -y
RUN apt-get install oracle-java7-installer -y
RUN apt-get install oracle-java7-set-default -y

ENV JAVA_HOME /usr/bin/java
ENV PATH $JAVA_HOME:$PATH

## END