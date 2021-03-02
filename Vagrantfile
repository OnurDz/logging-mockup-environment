# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    
    config.vm.define "cloud" do |cloud|
        cloud.vm.box = "ubuntu/bionic64"
        cloud.vm.hostname = "mlog-cloud"
        cloud.vm.network "private_network", ip: "11.0.0.10"
        #cloud.vm.network "forwarded_port", guest: 9200, host: 9211
        cloud.disksize.size = '20GB'
        cloud.vm.provider "virtualbox" do |vb|
            vb.name = "mlog_cloud"
            vb.memory = "4096"
        end
        cloud.vm.provision "shell", inline: <<-SHELL
            command -v python
            if [ $? -eq 1 ] ; then
                apt-get update
                apt-get install -y python
            fi
        SHELL
        cloud.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/cloud-playbook.yml"
        end
    end
    
    config.vm.define "master" do |master|
        master.vm.box = "ubuntu/bionic64"
        master.vm.hostname = "mlog-master"
        master.vm.network "private_network", ip: "10.0.0.11"
        master.disksize.size = '20GB'
        master.vm.provider "virtualbox" do |vb|
            vb.name = "mlog_master"
            vb.memory = "2048"
        end
        master.vm.provision "shell", inline: <<-SHELL
            command -v python
            if [ $? -eq 1 ] ; then
                apt-get update
                apt-get install -y python
            fi
        SHELL
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/master-playbook.yml"
        end
    end

    config.vm.define "puppet" do |puppet|
        puppet.vm.box = "ubuntu/bionic64"
        puppet.vm.hostname = "mlog-puppet"
        puppet.vm.network "private_network", ip: "10.0.0.10"
        puppet.vm.provider "virtualbox" do |vb|
            vb.name = "mlog_puppet"
            vb.memory = "1024"
        end
        puppet.vm.provision "shell", inline: <<-SHELL
            command -v python
            if [ $? -eq 1 ] ; then
                apt-get update
                apt-get install -y python
            fi
        SHELL
        puppet.vm.provision "ansible" do |ansible|
            ansible.playbook = "ansible/puppet-playbook.yml"
        end
    end

end
