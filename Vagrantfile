# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "jenkinsci" do |jenkins_config|
    jenkins_config.vm.hostname = "jenkins-server.test"
    jenkins_config.vm.network :private_network, ip: "192.168.56.56"
    jenkins_config.vm.box = "ubuntu/focal64"
    jenkins_config.vm.synced_folder ".", "/vagrant", disabled: true
    jenkins_config.ssh.insert_key = false
    jenkins_config.vm.provider "virtualbox" do |v|
      # v.gui = true
      v.cpus = 1
      v.memory = 512
    end

    jenkins_config.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y docker.io
      sudo usermod -aG docker vagrant

      # Install Java (required for Jenkins)
      sudo apt update
      sudo apt install fontconfig
      sudo apt install openjdk-17-jre-headless
      java --version
      # Install Jenkins
      sudo wget -q -O /usr/share/keyrings/jenkins-keyring.asc \
        https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
      echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
        https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
        /etc/apt/sources.list.d/jenkins.list > /dev/null
      sudo apt-get update
      sudo apt-get install -y jenkins
      sudo systemctl enable jenkins
      sudo systemctl start jenkins
      sudo systemctl status jenkins
    SHELL
    end
  end
end
 