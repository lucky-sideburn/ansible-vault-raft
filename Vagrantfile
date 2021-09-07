# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure('2') do |config|
  
  hosts = {}

  inventory_path = 'inventories/vagrant_dev.yml'
  hosts['vault1'] = 'vault01'
  hosts['vault2'] = 'vault02'
  hosts['vault3'] = 'vault03'

  $script = <<-SCRIPT
    sed -i 's/PasswordAuthentication\ no/PasswordAuthentication\ yes/g' /etc/ssh/sshd_config
    systemctl restart sshd
    ls /data || mkdir /data
    grep '192.168.58.71 #{hosts['vault1']}' /etc/hosts || echo '192.168.58.71 #{hosts['vault1']}' >> /etc/hosts
    grep '192.168.58.72 #{hosts['vault2']}' /etc/hosts || echo '192.168.58.72 #{hosts['vault2']}' >> /etc/hosts
    grep '192.168.58.73 #{hosts['vault3']}' /etc/hosts || echo '192.168.58.73 #{hosts['vault3']}' >> /etc/hosts
  SCRIPT
  config.vm.provision 'shell',
                      inline: $script
        
  config.vm.define hosts['vault1'] do |vault|
    vault.vm.box = 'geerlingguy/centos8'
    vault.vm.hostname = hosts['vault1']
    vault.vm.network 'private_network', ip: '192.168.58.71'
    vault.vm.provider :virtualbox do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end
    vault.vm.provision :ansible do |ansible|
      ansible.limit = hosts['vault1']
      ansible.playbook = 'provisioning/vault.yml'
      ansible.verbose = 'true'
      ansible.inventory_path = inventory_path
    end
  end

  config.vm.define hosts['vault2'] do |vault|
    vault.vm.box = 'geerlingguy/centos8'
    vault.vm.hostname = hosts['vault2']
    vault.vm.network 'private_network', ip: '192.168.58.72'
    vault.vm.provider :virtualbox do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end
    vault.vm.provision :ansible do |ansible|
      ansible.limit = hosts['vault2']
      ansible.playbook = 'provisioning/vault.yml'
      ansible.verbose = 'true'
      ansible.inventory_path = inventory_path
    end
  end

  config.vm.define hosts['vault3'] do |vault|
    vault.vm.box = 'geerlingguy/centos8'
    vault.vm.hostname = hosts['vault3']
    vault.vm.network 'private_network', ip: '192.168.58.73'
    vault.vm.provider :virtualbox do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end
    vault.vm.provision :ansible do |ansible|
      ansible.limit = hosts['vault3']
      ansible.playbook = 'provisioning/vault.yml'
      ansible.verbose = 'true'
      ansible.inventory_path = inventory_path
    end
  end

  config.vm.define 'haproxy01' do |vault|
    vault.vm.box = 'geerlingguy/centos8'
    vault.vm.hostname = 'haproxy01'
    vault.vm.network 'private_network', ip: '192.168.58.74'
    vault.vm.provider :virtualbox do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
    vault.vm.provision :ansible do |ansible|
      ansible.limit = 'haproxy01'
      ansible.playbook = 'provisioning/haproxy.yml'
      ansible.verbose = 'true'
      ansible.inventory_path = inventory_path
    end
  end
end
