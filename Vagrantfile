# -*- mode: ruby -*-
# vi: set ft=ruby :

private_key_path = File.join(Dir.home, ".ssh", "id_rsa")
public_key_path = File.join(Dir.home, ".ssh", "id_rsa.pub")
insecure_key_path = File.join(Dir.home, ".vagrant.d", "insecure_private_key")
private_key = IO.read(private_key_path)
public_key = IO.read(public_key_path)

Vagrant.configure("2") do |config|
  config.vm.define "centos6" do |centos6|
    centos6.vm.box = "centos/6"
    centos6.vm.hostname = "centos6"
    centos6.vm.network "forwarded_port", guest: 22, host: 2206
    centos6.vm.provision :shell, :inline => <<-SCRIPT
      yum install epel-release -y
    SCRIPT
  end

  config.vm.define "centos7" do |centos7|
    centos7.vm.box = "centos/7"
    centos7.vm.hostname = "centos7"
    centos7.vm.network "forwarded_port", guest: 22, host: 2207
    centos7.vm.provision :shell, :inline => <<-SCRIPT
      yum install epel-release -y
    SCRIPT
  end

  config.ssh.insert_key = false
  config.ssh.private_key_path = [
    private_key_path,
    insecure_key_path # to provision the first time
  ]

  config.vm.provision :shell, :inline => <<-SCRIPT
    set -e
    echo '#{private_key}' > /home/vagrant/.ssh/id_rsa
    chmod 600 /home/vagrant/.ssh/id_rsa
    echo '#{public_key}' > /home/vagrant/.ssh/authorized_keys
    chmod 600 /home/vagrant/.ssh/authorized_keys
  SCRIPT
end
