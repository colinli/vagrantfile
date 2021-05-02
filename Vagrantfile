# -*- mode: ruby -*-
# vi: set ft=ruby :

# ========= Definations =============
# SSH Key
use_own_key = true
private_key_name = "id_rsa"
public_key_name = "id_rsa.pub"

# Custom privision script
common_script = "
  echo Replace with your own script here
  echo by adding sudo for privileges
  echo and line by line"

script1 = "echo script on node1"
script2 = "echo script on node2"

# VM spec
default_box = "bento/centos-7"
default_cpus = 1
default_mem = 1024

nodes = {
  "controller" => {
     :box => default_box,
     :ip => "192.168.40.21",
     :mem => default_mem,
     :cpus => default_cpus,
     :script => script1},
  "worker" => {
     :box => default_box,
     :ip => "192.168.40.22",
     :mem => 512,
     :cpus => default_cpus,
     :script => script2}
}

plugins = ["vagrant-vbguest"]

# ============= Don't Change below =============

# Set ssh key
private_key_path = File.join(Dir.home, ".ssh", private_key_name)
public_key_path = File.join(Dir.home, ".ssh", public_key_name)
insecure_key_path = File.join(Dir.home, ".vagrant.d", "insecure_private_key")
private_key = IO.read(private_key_path)
public_key = IO.read(public_key_path)

set_key = "set -e
  echo Now setting up your own key
  echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
  chmod 600 /home/vagrant/.ssh/authorized_keys"


Vagrant.configure('2') do |config|
  nodes.each_with_index do | (hostname, spec), index|
    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |v, override|
        config.vm.box = spec[:box]
        override.vm.network :private_network, ip: spec[:ip]
        override.vm.hostname = hostname
        override.vm.provision 'shell', inline: spec[:script]
        v.name = hostname
        v.memory = spec[:mem]
        v.cpus = spec[:cpus]
      end #cfg
    end # config

    plugins.each do |plugin|
      system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
    end # plugins
  end #nodes

  config.ssh.insert_key = false
  config.ssh.private_key_path = [
    private_key_path,
    insecure_key_path
  ]

# Provision
  config.vm.provision 'shell', privileged: false, inline:
    set_key if use_own_key

  config.vm.provision 'shell', privileged: false, inline:
    common_script
end #Vagrant
