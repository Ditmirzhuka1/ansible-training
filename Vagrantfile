Vagrant.configure("2") do |config|
    
    config.vm.define "ansible_server" do |main_server|
        main_server.vm.box = "centos/7"
        main_server.vm.provision "file", source: "./id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
        main_server.vm.provision "shell", inline: <<-SHELL
            chmod 600 /home/vagrant/.ssh/id_rsa
            chown vagrant:vagrant /home/vagrant/.ssh/id_rsa
        SHELL
    end

    ip_suffix = 11
    ["apache1", "apache2", "extra1", "extra2"].each do |machine|
        config.vm.define machine do |node|
            node.vm.box = "generic/debian11"
            ip_suffix += 1
            node.vm.hostname = machine
            node.vm.network :private_network, ip:"192.168.61.#{ip_suffix}"
            node.vm.provision "file", source: "./id_rsa.pub", destination: "/home/vagrant/id_rsa.pub"
            node.vm.provision "shell", inline: <<-SHELL
                cat /home/vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
                chmod 600 /home/vagrant/.ssh/authorized_keys
                chown vagrant:vagrant /home/vagrant/.ssh/authorized_keys
            SHELL
        end
    end
end
