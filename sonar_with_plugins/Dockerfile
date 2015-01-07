FROM jahroots/sonarqube

MAINTAINER Julien Banse, "julien.banse@gmail.com"

RUN apt-get update && apt-get clean
RUN apt-get install -q -y wget

# download and install sonar plugins
RUN wget -P /opt/sonarqube-4.5/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/android/sonar-android-plugin/1.0/sonar-android-plugin-1.0.jar
RUN wget -P /opt/sonarqube-4.5/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/sonar-build-breaker-plugin/1.1/sonar-build-breaker-plugin-1.1.jar
RUN wget -P /opt/sonarqube-4.5/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-checkstyle-plugin/2.2/sonar-checkstyle-plugin-2.2.jar
RUN wget -P /opt/sonarqube-4.5/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-java-plugin/2.7/sonar-java-plugin-2.7.jar
RUN wget -P /opt/sonarqube-4.5/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/l10n/sonar-l10n-fr-plugin/1.10/sonar-l10n-fr-plugin-1.10.jar
RUN wget -P /opt/sonarqube-4.5/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/java/sonar-pmd-plugin/2.3/sonar-pmd-plugin-2.3.jar
RUN wget -P /opt/sonarqube-4.5/extensions/downloads/ http://repository.codehaus.org/org/codehaus/sonar-plugins/sonar-widget-lab-plugin/1.6/sonar-widget-lab-plugin-1.6.jar


## END