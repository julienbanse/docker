Docker SonarQube
======
The docker file base on docker [nimmis/java:oracle-7-jdk](https://registry.hub.docker.com/u/nimmis/java/) 

Default sonar version is 5.1.

This docker file need a mysql docker to be linked.

Some environnment variable are available to be more flexible.
These default values are defined :

`
SONAR_JDBC_URL jdbc:mysql://db:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
SONAR_DB_USERNAME sonar
SONAR_DB_PASSWORD 123qwe
SONARQUBE_VERSION 5.1
`

# When using in a vagrant file :

`
config.vm.provision "docker" do |d|
		d.run "julienbanse/sonar_with_plugins",
			args: "-d -p 9000:9000 -name ubuntu -link mysql:db"
	end
`
optionnal part of sonar are available : 9001 and 9010

# These plugins are installed by default :

- sonar-android-plugin-1.0
- sonar-build-breaker-plugin-1.1
- sonar-checkstyle-plugin-2.2
- sonar-java-plugin-3.1
- sonar-l10n-fr-plugin-1.13
- sonar-pmd-plugin-2.3
- sonar-widget-lab-plugin-1.6
- sonar-findbugs-plugin-3.2