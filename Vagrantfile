# -*- mode: ruby -*-
# vi: set ft=ruby :

cluster = {
  "master" => { :ip => "192.168.99.200", :cpus => 2, :mem => 4096 },
  "slave" => { :ip => "192.168.99.201", :cpus => 4, :mem => 4096 }
}
Vagrant.configure("2") do |config|
    cluster.each_with_index do |(hostname, info), index|
        config.vm.define hostname do |cfg|
                cfg.vm.provider :virtualbox do |vb, override|
                    config.vm.box = "bento/centos-7.7"
                    override.vm.network :private_network, ip: "#{info[:ip]}"
                    override.vm.hostname = hostname
                    vb.name = hostname
                    vb.customize ["modifyvm", :id, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on"]

                    file_to_disk = hostname+"-disk.vmdk"
                    unless File.exist?(file_to_disk)
                        vb.customize [ "createmedium", "disk", "--filename", file_to_disk, "--format", "vmdk", "--size", 1024 * 10 ]
                    end
                    vb.customize [ "storageattach", :id , "--storagectl", 
                        "SATA Controller", "--port", "1", "--device", "0", "--type", 
                        "hdd", "--medium", file_to_disk]
                end # end provider
        end # end config  
    end # end cluster
end