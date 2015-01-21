Docker Jenkins with android sdk
======
The docker file base on latest docker [jenkins](https://registry.hub.docker.com/_/jenkins/) 

You can connect to machine through ssh (in case jenkins is the docker name):

`
vagrant ssh jenkins`

user is root and password is root

# When using in a vagrant file :

`
config.vm.provision "docker" do |d|
		d.run "julienbanse/jenkins_with_android_sdk",
			args: "-d -p 8080:8080 -name jenkins"
	end
`

optionnal port of jenkins is available : 50000 (used for slave)

# These jenkins plugins are installed by default :

- credentials
- ssh-credentials
- promoted-builds
- javadoc
- mailer
- token-macro
- maven-plugin
- scm-api
- subversion
- run-condition
- conditional-buildstep
- parameterized-trigger
- git-client
- dashboard-view
- ant
- analysis-core
- git
- gradle
- analysis-collector
- checkstyle
- findbugs
- pmd
- htmlpublisher