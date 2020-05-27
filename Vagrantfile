# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.synced_folder "./", "/Vagrant"

  config.vm.define "gitlab" do |gitlab|
    gitlab.vm.hostname = "gitlab"
    gitlab.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
    gitlab.vm.network "private_network", ip: "192.168.33.10"

    gitlab.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
    end

    gitlab.vm.provision "shell", inline: <<-SHELL
      sudo yum install -y curl policycoreutils-python openssh-server
      sudo systemctl enable sshd
      sudo systemctl start sshd
      # sudo firewall-cmd --permanent --add-service=http
      # sudo firewall-cmd --permanent --add-service=https
      # sudo systemctl reload firewalld

      sudo yum install -y postfix
      sudo systemctl enable postfix
      sudo systemctl start postfix

      curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash

      sudo EXTERNAL_URL="http://gitlab.example.com" yum install -y gitlab-ee

      sudo gitlab-ctl reconfigure
    SHELL
  end

  config.vm.define "gitlab2" do |gitlab2|
    gitlab2.vm.hostname = "gitlab2"
    gitlab2.vm.network "forwarded_port", guest: 80, host: 10080, host_ip: "127.0.0.1"
    gitlab2.vm.network "private_network", ip: "192.168.33.11"

    gitlab2.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
    end

    gitlab2.vm.provision "shell", inline: <<-SHELL
      sudo yum install -y curl policycoreutils-python openssh-server
      sudo systemctl enable sshd
      sudo systemctl start sshd
      # sudo firewall-cmd --permanent --add-service=http
      # sudo firewall-cmd --permanent --add-service=https
      # sudo systemctl reload firewalld

      sudo yum install -y postfix
      sudo systemctl enable postfix
      sudo systemctl start postfix

      curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash

      sudo EXTERNAL_URL="http://gitlab.example.com" yum install -y gitlab-ee

      sudo gitlab-ctl reconfigure
    SHELL
  end

  config.vm.define "ldap" do |ldap|
    ldap.vm.hostname = "ldap"
    ldap.vm.network "forwarded_port", guest: 389, host: 10389, host_ip: "127.0.0.1"
    ldap.vm.network "private_network", ip: "192.168.33.20"

    ldap.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
    end

    ldap.vm.provision "shell", inline: <<-SHELL
      yum install -y openldap openldap-clients openldap-servers
      cp -a /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
      systemctl enable slapd
      systemctl start slapd
    SHELL
  end
end
