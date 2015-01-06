Docker SonarQube 4.5.1 LTS
======
The docker file base on julienbanse/ubuntu.14.04_with_java

This docker file need a mysql docker to be linked.

Somme environnment variable are available to be more flexible :

`
SONAR_JDBC_URL jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
SONAR_DB_USERNAME sonar
SONAR_DB_PASSWORD sonar
SONARQUBE_VERSION 4.5.1
`

# When using in a vagrant file :

`
config.vm.provision "docker" do |d|
		d.run "julienbanse/sonar_with_plugins",
			args: "-d -p 9000:9000 -name ubuntu -link mysql:db"
	end
`

# These plugins are installed by default :

- sonar-android-plugin-1.0
- sonar-build-breaker-plugin-1.1
- sonar-checkstyle-plugin-2.2
- sonar-java-plugin-2.7
- sonar-l10n-fr-plugin-1.10
- sonar-pmd-plugin-2.3
- sonar-widget-lab-plugin-1.6