FROM ubuntu:14.04
MAINTAINER Julien Banse, "julien.banse@gmail.com"

RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu precise main" | tee /etc/apt/sources.list.d/webupd8team-java.list
RUN echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu precise main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886

RUN apt-get update && apt-get clean
RUN apt-get install -q -y wget
RUN apt-get install -q -y zip
RUN apt-get install -q -y openssh-server
RUN apt-get install -q -y supervisor

# Never ask for confirmations
ENV DEBIAN_FRONTEND noninteractive
RUN echo "debconf shared/accepted-oracle-license-v1-1 select true" | /usr/bin/debconf-set-selections
RUN echo "debconf shared/accepted-oracle-license-v1-1 seen true" | /usr/bin/debconf-set-selections

# Add oracle-jdk8 to repositories
RUN apt-get install python-software-properties -y
RUN apt-get install oracle-java8-installer -y
RUN apt-get install oracle-java8-set-default -y

ENV JAVA_HOME /usr/bin/java
ENV PATH $JAVA_HOME:$PATH

# Configure ssh
RUN mkdir -p /var/run/sshd
RUN chmod 755 /var/run/sshd
RUN echo 'root:root' |chpasswd
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config
RUN sed -ri 's/PermitRootLogin without-password/PermitRootLogin yes/g' /etc/ssh/sshd_config

# Configure Supervisor
RUN mkdir -p /var/log/supervisor

# Expose ports
EXPOSE 22

# Start Supervisor
CMD ["/usr/bin/supervisord","-n"]

## END