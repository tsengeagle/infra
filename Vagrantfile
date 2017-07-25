# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vm.provider "vmware_workstation" do |v|
    v.gui = true
    v.vmx["memsize"] = "4096"
    v.vmx["numvcpus"] = "2"
  end

  config.ssh.private_key_path = ["~/.ssh/id_rsa","~/.vagrant.d/insecure_private_key"]
  config.ssh.insert_key = false
  config.vm.provision "file", source: "./authorized_keys", destination: "~/.ssh/authorized_keys"

  config.vm.network "public_network"

  config.vm.define "gitlab" do |gitlab|
    gitlab.vm.hostname = "gitlab"

    gitlab.vm.provision "shell", inline: <<-EOC
        sudo yum install curl policycoreutils openssh-server openssh-clients -y
        sudo systemctl enable sshd
        sudo systemctl start sshd
        sudo yum install postfix -y
        sudo systemctl enable postfix
        sudo systemctl start postfix
        sudo firewall-cmd --permanent --add-service=http
        sudo systemctl reload firewalld
        curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
        sudo yum install gitlab-ce -y
        sudo gitlab-ctl reconfigure
    EOC
  end


end
