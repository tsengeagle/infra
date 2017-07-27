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
  config.vm.provision "shell", inline: <<-EOC
    sudo yum update -y
    sudo yum install wget -y
  EOC
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
        #curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
        #sudo yum install gitlab-ee -y
        sudo gitlab-ctl reconfigure
    EOC
  end

  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.hostname = "jenkins"

    jenkins.vm.provision "shell", inline: <<-EOC
      sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
      sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
      sudo yum install -y java
      sudo yum install -y jenkins
      sudo chkconfig jenkins on
      sudo systemctl start jenkins.service
    EOC
  end

  config.vm.define "dw01" do |dw01|
    dw01.vm.hostname = "dw01"
    dw01.vm.provision "shell", inline: <<-EOC
      sudo yum install -y postgresql postgresql-server postgresql-contrib
      sudo postgresql-setup initdb
    EOC
  end

end
