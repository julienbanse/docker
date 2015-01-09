FROM nimmis/java:oracle-7-jdk
MAINTAINER Julien Banse, "julien.banse@gmail.com"

RUN apt-get update && apt-get clean

RUN apt-get install -q -y wget
RUN apt-get install -q -y zip
RUN apt-get install -q -y openssh-server
RUN apt-get install -q -y supervisor

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
#
RUN wget http://mirrors.jenkins-ci.org/war/latest/jenkins.war -O /opt/jenkins.war
RUN ln -sf /jenkins /root/.jenkins

# Add Jenkins plugins
#
# Jenkins: Jobs
RUN mkdir -p /data/jenkins/jobs/Template_Android_Gradle
ADD Template_Android_Gradle/config.xml /data/jenkins/jobs/Template_Android_Gradle/

#plugins
RUN mkdir -p /data/jenkins/plugins
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/credentials.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/ssh-credentials.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/promoted-builds.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/javadoc.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/mailer.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/token-macro.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/maven-plugin.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/scm-api.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/subversion.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/run-condition.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/conditional-buildstep.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/parameterized-trigger.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/git-client.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/dashboard-view.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/ant.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/analysis-core.hpi

# Jenkins: Get Plugins
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/git.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/gradle.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/analysis-collector.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/checkstyle.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/findbugs.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/pmd.hpi
RUN wget -P /data/jenkins/plugins/ https://updates.jenkins-ci.org/latest/htmlpublisher.hpi

### Clean
RUN apt-get -y autoclean
RUN apt-get -y clean
RUN apt-get -y autoremove

### Configure Supervisor
RUN mkdir -p /var/log/supervisor
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 8080 22

VOLUME ["/jenkins"]

### Start Supervisor (with jenkins and sshd)
CMD ["/usr/bin/supervisord","-n"]

## END