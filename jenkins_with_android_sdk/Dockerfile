FROM jenkins
MAINTAINER Julien Banse, "julien.banse@gmail.com"

# Add Jenkins plugins
# plugins
COPY plugins.txt /plugins.txt
RUN /usr/local/bin/plugins.sh /plugins.txt

# Jenkins: Jobs
RUN mkdir -p ${JENKINS_HOME}/jobs/Template_Android_Gradle
ADD Template_Android_Gradle/config.xml ${JENKINS_HOME}/jobs/Template_Android_Gradle/

USER root
RUN apt-get update && apt-get install -y expect

#android
RUN wget http://dl.google.com/android/android-sdk_r24-linux.tgz -O /opt/android-sdk_r24-linux.tgz
RUN tar xf /opt/android-sdk_r24-linux.tgz -C /opt/

ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

RUN mkdir -p /opt/tools
COPY sdk_android.sh /opt/tools/android-accept-licenses.sh
ENV PATH ${PATH}:/opt/tools

RUN chmod 755 /opt/tools/android-accept-licenses.sh

RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter tools --no-ui --all"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter extra --no-ui --all"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter platform-tools --no-ui"]
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter android-21 --no-ui"]

RUN chmod -R 755 /opt/android-sdk-linux

### Clean
RUN apt-get -y autoclean
RUN apt-get -y clean
RUN apt-get -y autoremove

## END