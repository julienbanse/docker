FROM jenkins
MAINTAINER Julien Banse, "julien.banse@gmail.com"

# Add Jenkins plugins
# plugins
COPY plugins.txt /plugins.txt
RUN /usr/local/bin/plugins.sh /plugins.txt

USER root
 
RUN apt-get update && apt-get install -y openssh-server supervisor

#android
RUN wget http://dl.google.com/android/android-sdk_r24-linux.tgz -O /opt/android-sdk_r24-linux.tgz
RUN tar xvf /opt/android-sdk_r24-linux.tgz -C /opt/
ENV ANDROID_HOME /opt/android-sdk-linux
RUN chmod -R 777 $ANDROID_HOME

ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

ADD sdk_android.sh /opt/tools/android-accept-licenses.sh
ENV PATH ${PATH}:/opt/tools

RUN chmod 755 /opt/tools/android-accept-licenses.sh

RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --no-ui"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter android-21 --no-ui --all"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter extra --no-ui --all"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter platform-tools,build-tools-21.1.2 --no-ui --all"]

# Jenkins: Jobs
#RUN mkdir -p /data/jenkins/jobs/Template_Android_Gradle
#ADD Template_Android_Gradle/config.xml /data/jenkins/jobs/Template_Android_Gradle/

### Configure ssh
RUN mkdir -p /var/run/sshd
RUN chmod 755 /var/run/sshd
RUN echo 'root:root' |chpasswd
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config
RUN sed -ri 's/PermitRootLogin without-password/PermitRootLogin yes/g' /etc/ssh/sshd_config

### Clean
RUN apt-get -y autoclean
RUN apt-get -y clean
RUN apt-get -y autoremove

### Configure Supervisor
RUN mkdir -p /var/log/supervisor
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 22

#VOLUME ["/jenkins"]

### Start Supervisor (with jenkins and sshd)
CMD ["/usr/bin/supervisord","-n"]

## END