FROM julienbanse/ubuntu.14.04_with_java7
MAINTAINER Julien Banse, "julien.banse@gmail.com"

RUN apt-get update && apt-get clean
RUN apt-get install -q -y wget
RUN apt-get install -q -y zip

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

ENTRYPOINT ["java", "-jar", "/opt/jenkins.war"]
EXPOSE 8080
VOLUME ["/jenkins"]
CMD [""]

# Add Jenkins plugins
#
# Jenkins: Jobs
#RUN mkdir -p /data/jenkins/jobs/Template_Android_Gradle
#RUN cp ./Template_Android_Gradle/config.xml /data/jenkins/jobs/Template_Android_Gradle/.

RUN mkdir -p /data/jenkins/plugins
RUN cd /data/jenkins/plugins
RUN wget https://updates.jenkins-ci.org/latest/credentials.hpi
RUN wget https://updates.jenkins-ci.org/latest/ssh-credentials.hpi
RUN wget https://updates.jenkins-ci.org/latest/promoted-builds.hpi
RUN wget https://updates.jenkins-ci.org/latest/javadoc.hpi
RUN wget https://updates.jenkins-ci.org/latest/mailer.hpi
RUN wget https://updates.jenkins-ci.org/latest/token-macro.hpi
RUN wget https://updates.jenkins-ci.org/latest/maven-plugin.hpi
RUN wget https://updates.jenkins-ci.org/latest/scm-api.hpi
RUN wget https://updates.jenkins-ci.org/latest/subversion.hpi
RUN wget https://updates.jenkins-ci.org/latest/run-condition.hpi
RUN wget https://updates.jenkins-ci.org/latest/conditional-buildstep.hpi
RUN wget https://updates.jenkins-ci.org/latest/parameterized-trigger.hpi
RUN wget https://updates.jenkins-ci.org/latest/git-client.hpi
RUN wget https://updates.jenkins-ci.org/latest/dashboard-view.hpi
RUN wget https://updates.jenkins-ci.org/latest/ant.hpi
RUN wget https://updates.jenkins-ci.org/latest/analysis-core.hpi

# Jenkins: Plugins
RUN wget https://updates.jenkins-ci.org/latest/git.hpi
RUN wget https://updates.jenkins-ci.org/latest/gradle.hpi
RUN wget https://updates.jenkins-ci.org/latest/analysis-collector.hpi
RUN wget https://updates.jenkins-ci.org/latest/checkstyle.hpi
RUN wget https://updates.jenkins-ci.org/latest/findbugs.hpi
RUN wget https://updates.jenkins-ci.org/latest/pmd.hpi
RUN wget https://updates.jenkins-ci.org/latest/htmlpublisher.hpi

## END