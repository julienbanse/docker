# -*- mode: ruby -*-
# vi: set ft=ruby :
# vagrant file to launch docker containers to have a continuous integration for android. But this can be used for other app analyses.

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

SONAR_JDBC_URL = 'jdbc:mysql://db:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance'
SONAR_DB_USERNAME = 'sonar'
SONAR_DB_PASSWORD = '123qwe'

VAGRANTFILE_API_VERSION = 2

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|	
	
	#default user for ssh is vagrant, we modify it to root because ssh server on docker machine is setup with root 
	config.ssh.username = "root"
	
	#launch mysql docker with a default db schema useable for sonar.
	config.vm.define "mysql" do |v|
		v.vm.provider "docker" do |d|
			d.image = "tpires/sonar-mysql"
			d.name = "mysql"
			d.ports = ["3306:3306"]
			d.remains_running = true
			d.vagrant_machine = "android_CI"
			d.vagrant_vagrantfile = "./DockerHostVagrantfile"
		end
	end
	
	#launch sonar docker and plug it with previous mysql docker with link command.
	# you can connect to this machine with command : vagrant ssh sonar (as root user and password root)
	config.vm.define "sonar" do |v|
    v.vm.provider "docker" do |d|
	    d.image = "julienbanse/sonar_with_plugins"
	    d.ports = ["9000:9000", "9001:9001", "9010:9010"]
	    d.name = "sonar"
	    d.link("mysql:db")
	    d.remains_running = true
	    d.vagrant_machine = "android_CI"
	    d.vagrant_vagrantfile = "./DockerHostVagrantfile"
	    d.env = {SONAR_JDBC_URL:"#{SONAR_JDBC_URL}",SONAR_DB_USERNAME:"#{SONAR_DB_USERNAME}",SONAR_DB_PASSWORD:"#{SONAR_DB_PASSWORD}"}
    end
  end

	#launch jenkins docker. This docker contains android sdk.
	# you can connect to this machine with command : vagrant ssh jenkins (as root user and password root)
	config.vm.define "jenkins" do |v|
		v.vm.provider "docker" do |d|
			d.image = "julienbanse/jenkins_with_android_sdk:latest"
			d.ports = ["8080:8080", "50000:50000"]
			d.name = "jenkins"
			d.volumes = ["/var/jenkins_home"]
			d.remains_running = true
			d.vagrant_machine = "android_CI"
			d.vagrant_vagrantfile = "./DockerHostVagrantfile"
		end
	end

end
