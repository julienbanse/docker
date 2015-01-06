Docker with Ubuntu 14.04 LTS and Java
======
The docker file is based on Ubuntu 14.04 LTS

SSH connection is enabled with root:root

Java 8 is installed

When using in a vagrant file :

``
config.vm.provision "docker" do |d|
		d.run "julienbanse/jenkins_with_android_sdk",
			args: "-d -p 22:22 -name ubuntu"
	end
``
