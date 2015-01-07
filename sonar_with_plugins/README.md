Docker SonarQube 4.5
======
The docker file base on docker [jahroots/sonarqube](https://registry.hub.docker.com/u/jahroots/sonarqube/) 

This docker file need a mysql docker to be linked.

# When using in a vagrant file :

`
config.vm.provision "docker" do |d|
		d.run "julienbanse/sonar_with_plugins",
			args: "-d -p 9000:9000 -name sonar"
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