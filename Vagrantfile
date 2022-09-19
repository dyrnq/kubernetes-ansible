# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/jammy64"

    config.vm.box_check_update = false
    config.ssh.insert_key = false
    # insecure_private_key download from https://github.com/hashicorp/vagrant/blob/master/keys/vagrant
    config.ssh.private_key_path = "insecure_private_key"

    machines = {
        'etcd-1' => '192.168.27.3',
        'etcd-2' => '192.168.27.4',
        'etcd-3' => '192.168.27.5',        
        # kube control planenodes
        'master-1' => '192.168.27.11',
        'master-2' => '192.168.27.12',
        'master-3' => '192.168.27.13',
        # kube worker nodes
        'worker-1' => '192.168.27.111',
        'worker-2' => '192.168.27.112',
        'worker-3' => '192.168.27.113'
    }

    machines.each do |name, ip|
        config.vm.define name do |machine|
            machine.vm.network "private_network", ip: ip

            machine.vm.hostname = name
            machine.vm.provider :virtualbox do |vb|
                vb.name = name  
                vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
                vb.customize ["modifyvm", :id, "--vram", "32"]
                vb.customize ["modifyvm", :id, "--ioapic", "on"]
                vb.customize ["modifyvm", :id, "--cpus", "2"]
                vb.customize ["modifyvm", :id, "--memory", "2048"]
            end


            machine.vm.provision "shell", inline: <<-SHELL
                echo "root:vagrant" | sudo chpasswd
                timedatectl set-timezone "Asia/Shanghai"
            SHELL

        end
    end
end