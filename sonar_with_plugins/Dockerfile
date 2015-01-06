FROM julienbanse/ubuntu.14.04_with_java
MAINTAINER Julien Banse, "julien.banse@gmail.com"

RUN apt-get update && apt-get clean
RUN apt-get install -q -y wget
RUN apt-get install -q -y zip

# sonar
ENV SONAR_JDBC_URL jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
ENV SONAR_DB_USERNAME sonar
ENV SONAR_DB_PASSWORD sonar
ENV SONARQUBE_VERSION 4.5.1

RUN wget http://dist.sonar.codehaus.org/sonarqube-${SONARQUBE_VERSION}.zip -O /tmp/sonarqube-${SONARQUBE_VERSION}.zip 
RUN unzip /tmp/sonarqube-${SONARQUBE_VERSION}.zip -d /opt
RUN ln -s /opt/sonarqube-${SONARQUBE_VERSION} /opt/sonarqube

RUN sed -i s*#sonar.jdbc.url=jdbc:h2:tcp://localhost:9092/sonar*sonar.jdbc.url=\${env:SONAR_JDBC_URL}*g /opt/sonarqube/conf/sonar.properties && \
    sed -i s*#sonar.jdbc.username=sonar*sonar.jdbc.username=\${env:SONAR_DB_USERNAME}*g /opt/sonarqube/conf/sonar.properties && \
    sed -i s*#sonar.jdbc.password=sonar*sonar.jdbc.password=\${env:SONAR_DB_PASSWORD}*g /opt/sonarqube/conf/sonar.properties

VOLUME ["/opt/sonarqube"]

#Expose sonar port
EXPOSE 9000

# download and install sonar plugins
RUN wget http://repository.codehaus.org/org/codehaus/sonar-plugins/android/sonar-android-plugin/1.0/sonar-android-plugin-1.0.jar -O /opt/sonarqube/extensions/downloads
RUN wget http://repository.codehaus.org/org/codehaus/sonar-plugins/sonar-build-breaker-plugin/1.1/sonar-build-breaker-plugin-1.1.jar -O /opt/sonarqube/extensions/downloads
RUN wget http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-checkstyle-plugin/2.2/sonar-checkstyle-plugin-2.2.jar -O /opt/sonarqube/extensions/downloads
RUN wget http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-java-plugin/2.7/sonar-java-plugin-2.7.jar -O /opt/sonarqube/extensions/downloads
RUN wget http://repository.codehaus.org/org/codehaus/sonar-plugins/l10n/sonar-l10n-fr-plugin/1.10/sonar-l10n-fr-plugin-1.10.jar -O /opt/sonarqube/extensions/downloads
RUN wget http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-pmd-plugin/2.3/sonar-pmd-plugin-2.3.jar -O /opt/sonarqube/extensions/downloads
RUN wget http://repository.codehaus.org/org/codehaus/sonar-plugins/sonar-widget-lab-plugin/1.6/sonar-widget-lab-plugin-1.6.jar -O /opt/sonarqube/extensions/downloads

#start sonarqube
CMD /opt/sonar/bin/linux-x86-64/sonar.sh console

## END