Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/xenial64"

    config.vm.define "dns" do |dns|
        dns.vm.network "public_network", bridge: "enp3s0", ip: "10.22.1.51"
    end
    config.vm.define "eatw" do |eatw|
        eatw.vm.network "public_network", bridge: "enp3s0", ip: "10.22.1.52"
    end

    config.vm.provider :virtualbox do |vb|
        vb.customize [
            "modifyvm", :id,
            "--cpuexecutioncap", "100",
            "--memory", "2048"]
    end

    config.vm.provision "shell", inline: "sudo apt-get install -yq python python3"
    config.vm.provision :ansible do |ansible|
        ansible.playbook = "provisioning/playbook.yaml"
        ansible.groups = {
            "group_dns" => ["dns"],
            "group_eatw" => ["eatw"],
            "all_groups:children" => ["dns", "eatw"],
            "group_dns:vars" => {
                "bind_zone_master_server_ip": "10.22.1.51",
            },
        }
    end
end
