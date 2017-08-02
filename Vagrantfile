# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vm.provider "vmware_workstation" do |v|
    v.gui = true
    v.vmx["memsize"] = "2048"
    v.vmx["numvcpus"] = "1"
  end

  config.ssh.private_key_path = ["~/.ssh/id_rsa","~/.vagrant.d/insecure_private_key"]
  config.ssh.insert_key = false
  config.vm.provision "file", source: "./authorized_keys", destination: "~/.ssh/authorized_keys"
  config.vm.provision "shell", inline: <<-EOC
    sudo yum update -y
    sudo yum install glibc.i686 wget epel-release unzip -y
  EOC

  config.vm.network "public_network"

  config.vm.define "controller" do |controller|
    controller.vm.hostname = "controller"

    controller.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"

    controller.vm.provision "shell", inline: <<-EOC
      sudo yum install ansible -y
    EOC

  end


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

  config.vm.define "ap" do |ap|
    ap.vm.hostname = "ap"

  end

  config.vm.define "ci-runner" do |ci_runner|
    ci_runner.vm.hostname = "ci-runner"

    ci_runner.vm.provision "shell", inline: <<-EOC
      
    EOC
  end

  config.vm.define "dw01" do |dw01|
    dw01.vm.hostname = "dw01"
  end

end
