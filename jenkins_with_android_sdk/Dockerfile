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

#android
RUN wget http://dl.google.com/android/android-sdk_r24-linux.tgz -O /opt/android-sdk_r24-linux.tgz
RUN tar xvf /opt/android-sdk_r24-linux.tgz -C /opt/
ENV ANDROID_HOME /opt/android-sdk-linux
RUN chmod -R 777 $ANDROID_HOME
ENV PATH $ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$PATH

RUN echo "y" | android update sdk --no-ui
RUN echo "y" | android update sdk --filter android-21 --no-ui --all
RUN echo "y" | android update sdk --filter extra --no-ui --all
RUN echo "y" | android update sdk --filter platform-tools,build-tools-21.1.2 --no-ui --all

# Add Jenkins
# Thanks to orchardup/jenkins Dockerfile
RUN wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
RUN echo "deb http://pkg.jenkins-ci.org/debian-stable binary/" >> /etc/apt/sources.list
RUN apt-get update
# HACK: https://issues.jenkins-ci.org/browse/JENKINS-20407
RUN mkdir /var/run/jenkins
RUN apt-get install -y jenkins
RUN service jenkins start
EXPOSE 8080
VOLUME ["/var/lib/jenkins"]
ENTRYPOINT [ "java","-jar","/usr/share/jenkins/jenkins.war" ]
## END