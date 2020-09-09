# -*- mode: ruby -*-
# vi: set ft=ruby :

private_key_path = File.join(Dir.home, ".ssh", "id_rsa")
public_key_path = File.join(Dir.home, ".ssh", "id_rsa.pub")
insecure_key_path = File.join(Dir.home, ".vagrant.d", "insecure_private_key")
private_key = IO.read(private_key_path)
public_key = IO.read(public_key_path)

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7"
  config.vm.define "node1" do |node1|
    node1.vm.hostname = "node1"
    node1.vm.network "private_network", ip: "192.168.99.101"
    node1.vm.network "forwarded_port", guest: 22, host: 2201
    node1.vm.provider "virtualbox" do |vb|
      vb.cpus = 4
      vb.memory = 4096
    end
  end
  config.vm.define "node2" do |node2|
    node2.vm.hostname = "node2"
    node2.vm.network "private_network", ip: "192.168.99.201"
    node2.vm.network "forwarded_port", guest: 22, host: 2202
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
