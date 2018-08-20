disk1 = "node1.vdi"
disk2 = "node2.vdi"

Vagrant.configure("2") do |config|
    config.vm.define "admin" do |admin|    
        admin.vm.box = "ubuntu/bionic64"
        admin.vm.network "private_network", ip: "10.2.2.2"
        admin.vm.hostname = "admin"
    end

    config.vm.define "node1" do |node1|    
        node1.vm.box = "ubuntu/bionic64"
        node1.vm.network "private_network", ip: "10.2.2.3"
        node1.vm.hostname = "node1"

        node1.vm.provider "virtualbox" do |vb|
            unless File.exist? (disk1)		    
                vb.customize ['createhd', '--filename', disk1, '--size', 10 * 1024]
            end
            vb.customize ['storageattach', :id, '--storagectl', 'SCSI', '--port', 2, '--device', 0, '--type',  'hdd', '--medium', disk1]
        end
    end

    config.vm.define "node2" do |node2|
        node2.vm.box = "ubuntu/bionic64"
        node2.vm.network "private_network", ip: "10.2.2.4"
        node2.vm.hostname = "node2"

        node2.vm.provider "virtualbox" do |vb|
            unless File.exist? (disk2)
                vb.customize ['createhd', '--filename', disk2, '--size', 10 * 1024]
            end
            vb.customize ['storageattach', :id, '--storagectl', 'SCSI', '--port', 2, '--device', 0, '--type',  'hdd', '--medium', disk2]
        end
    end

    config.vm.define "client" do |client|    
        client.vm.box = "ubuntu/bionic64"
        client.vm.network "private_network", ip: "10.2.2.5"
        client.vm.hostname = "client"
    end
end
