# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
	config.vm.provision "shell", inline: "echo Hello World"
	
	config.vm.define "ansible" do |ansible|

		ansible.vm.box = "centos/7"
		ansible.vm.box_version = "1809.01"
		ansible.vm.network "private_network", ip: "192.168.10.21"
		ansible.vm.hostname = "master"

		ansible.vm.provider :hyperv do |v|
			v.enable_virtualization_extensions = true
			v.maxmemory = 2048
			v.memory = 2048
			v.cpus = 2
		end

		ansible.vm.provision "shell", inline: <<-SHELL
			sudo hostnamectl set-hostname master.test.com

			sudo echo "192.168.10.22 client1.test.com client1" | sudo tee -a /etc/hosts
			sudo echo "192.168.10.23 client2.test.com client2" | sudo tee -a /etc/hosts

			sudo systemctl enable firewalld
			sudo systemctl start firewalld

			sudo yum -y install ntp
			sudo timedatectl set-timezone Europe/Warsaw
			sudo systemctl start ntpd
			sudo firewall-cmd --add-service=ntp --permanent
			
			sudo systemctl restart firewalld
			
			sudo yum install epel-release -y
			sudo yum install ansible -y
		
			cat /dev/zero | ssh-keygen -q -N ""	
			
		SHELL

	  end
	  
	config.vm.define "client1" do |client1|

		client1.vm.box = "centos/7"
		client1.vm.box_version = "1809.01"
		client1.vm.network "private_network", ip: "192.168.10.22"
		client1.vm.hostname = "client1"

		client1.vm.provider :hyperv do |v|
			v.enable_virtualization_extensions = true
			v.maxmemory = 1024
			v.memory = 1024
			v.cpus = 1
		end

		client1.vm.provision "shell", inline: <<-SHELL
			sudo hostnamectl set-hostname client1.test.com
			sudo echo "192.168.10.21 master.test.com master" | sudo tee -a /etc/hosts
			
			sudo systemctl enable firewalld
			sudo systemctl start firewalld
			
			sudo yum -y install ntp
			sudo timedatectl set-timezone Europe/Warsaw
			sudo systemctl start ntpd
			sudo firewall-cmd --add-service=ntp --permanent
			
			sudo systemctl restart firewalld

		SHELL

		end

	config.vm.define "client2" do |client2|

		client2.vm.box = "centos/7"
		client2.vm.box_version = "1809.01"
		client2.vm.network "private_network", ip: "192.168.10.23"
		client2.vm.hostname = "client2"

		client2.vm.provider :hyperv do |v|
			v.enable_virtualization_extensions = true
			v.maxmemory = 1024
			v.memory = 1024
			v.cpus = 1
		end

		client2.vm.provision "shell", inline: <<-SHELL
			sudo hostnamectl set-hostname client2.test.com
			sudo echo "192.168.10.21 master.test.com master" | sudo tee -a /etc/hosts
			
			sudo systemctl enable firewalld
			sudo systemctl start firewalld
			
			sudo yum -y install ntp
			sudo timedatectl set-timezone Europe/Warsaw
			sudo systemctl start ntpd
			sudo firewall-cmd --add-service=ntp --permanent
			
			sudo systemctl restart firewalld

		SHELL

  end
end