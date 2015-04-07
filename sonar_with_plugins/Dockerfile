FROM nimmis/java:oracle-7-jdk

MAINTAINER Julien Banse, "julien.banse@gmail.com"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get clean

RUN apt-get install -q -y wget
RUN apt-get install -q -y zip
RUN apt-get install -q -y expect

# sonar
ENV SONAR_JDBC_URL jdbc:mysql://db:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
ENV SONAR_DB_USERNAME sonar
ENV SONAR_DB_PASSWORD 123qwe
ENV SONARQUBE_VERSION 5.1

RUN wget http://dist.sonar.codehaus.org/sonarqube-${SONARQUBE_VERSION}.zip -O /tmp/sonarqube-${SONARQUBE_VERSION}.zip 
RUN unzip /tmp/sonarqube-${SONARQUBE_VERSION}.zip -d /opt
RUN ln -s /opt/sonarqube-${SONARQUBE_VERSION} /opt/sonarqube

RUN sed -i s*#sonar.jdbc.url=jdbc:h2:tcp://localhost:9092/sonar*sonar.jdbc.url=\${env:SONAR_JDBC_URL}*g /opt/sonarqube/conf/sonar.properties && \
    sed -i s*#sonar.jdbc.username=sonar*sonar.jdbc.username=\${env:SONAR_DB_USERNAME}*g /opt/sonarqube/conf/sonar.properties && \
    sed -i s*#sonar.jdbc.password=sonar*sonar.jdbc.password=\${env:SONAR_DB_PASSWORD}*g /opt/sonarqube/conf/sonar.properties

# download and install sonar plugins
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/android/sonar-android-plugin/1.0/sonar-android-plugin-1.0.jar
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/sonar-build-breaker-plugin/1.1/sonar-build-breaker-plugin-1.1.jar
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-checkstyle-plugin/2.2/sonar-checkstyle-plugin-2.2.jar
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-java-plugin/3.1/sonar-java-plugin-3.1.jar
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/l10n/sonar-l10n-fr-plugin/1.13/sonar-l10n-fr-plugin-1.13.jar
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-pmd-plugin/2.3/sonar-pmd-plugin-2.3.jar
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/sonar-widget-lab-plugin/1.6/sonar-widget-lab-plugin-1.6.jar
RUN wget -P /opt/sonarqube/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-findbugs-plugin/3.2/sonar-findbugs-plugin-3.2.jar

#android sdk for sonar-android analyses (lint tools)
RUN wget http://dl.google.com/android/android-sdk_r24-linux.tgz -O /opt/android-sdk_r24-linux.tgz
RUN tar xf /opt/android-sdk_r24-linux.tgz -C /opt/
ENV ANDROID_HOME /opt/android-sdk-linux
RUN chmod -R 777 /opt/android-sdk-linux

ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

RUN mkdir -p /opt/tools
COPY sdk_android.sh /opt/tools/android-accept-licenses.sh
ENV PATH ${PATH}:/opt/tools

RUN chmod 755 /opt/tools/android-accept-licenses.sh
RUN ["/opt/tools/android-accept-licenses.sh", "android update sdk --filter tools --no-ui --all"]
RUN chmod -R 777 /opt/android-sdk-linux

VOLUME ["/opt/sonarqube"]

#Expose sonar port
EXPOSE 9000 9001 9010

# Start Sonar
ENTRYPOINT /opt/sonarqube/bin/linux-x86-64/sonar.sh stop && /opt/sonarqube/bin/linux-x86-64/sonar.sh console

## END