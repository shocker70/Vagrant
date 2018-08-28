# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

#vagrant plugin install vagrant-vbguest
unless Vagrant.has_plugin?("vagrant-vbguest")
  raise 'Missing plugin, please run :  vagrant plugin install vagrant-vbguest'
end

Vagrant.configure("2") do |config|
	config.vm.box = "centos/7"
	config.vm.box_check_update = false
	config.vm.synced_folder "C:/Vagrant/vagrant_data" , "/home/vagrant/data" 
	config.vm.provision "shell", inline: $scriptdocker
end


$scriptdocker = <<-SCRIPT

	yum update
	yum install -y wget
	yum install -y unzip
	yum install -y git
	
	# Download terraform + install and move to /usr/bin/ 
	wget https://releases.hashicorp.com/terraform/0.11.8/terraform_0.11.8_linux_amd64.zip
	unzip terraform_*
	cp terraform /usr/bin/
	
	# Create symlink to folders in shared folder.
	# eg : ln -s /home/vagrant/data/terraform_info  /home/vagrant/terraform_init
	
	# Prerequisite for minikube: VBox
	wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo -P /etc/yum.repos.d/
	sudo rpm --import oracle_vbox.asc
	yum install -y VirtualBox-5.1
	
	
	# Install kubernetes
	cat <<-'EOF' > /etc/yum.repos.d/kubernetes.repo
	[kubernetes]
	name=Kubernetes
	baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
	enabled=1
	gpgcheck=1
	repo_gpgcheck=1
	gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
	EOF
	
	yum install -y kubernetes docker
	
	# Install minikube
       curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.2/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
	
	# Download Terraform examples	
	# cd /your/desired/folder/ && git clone https://github.com/terraform-providers/terraform-provider-aws.git
	
SCRIPT
