# Vagrant

Vagrantfile create 1 VM

```bash
VM_ID = "minhtuan"

Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-10"
  config.vm.box_check_update = true
  config.ssh.password = "vagrant"
  config.vm.define "#{VM_ID}" do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "#{VM_ID}"
      vb.memory = 2048
      vb.cpus = 2
    end
    node.vm.hostname = "#{VM_ID}"
    # node.vm.network :private_network, ip: "192.168.11.10"
    # node.vm.network :public_network, ip: "192.168.1.10", bridge: "Qualcomm Atheros QCA61x4A Wireless Network Adapter"
    node.vm.network :public_network, ip: "192.168.1.10"
  end
  config.vm.provision "shell",
    run: "always",
    inline: "apt-get install net-tools && route add default gw 192.168.1.2"
end
```

Vagrantfile create 3 VM

```bash
VM_ID = "minhtuan"
NUM_WORKER_NODES = 2

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_check_update = true

# Single Control-Plane node  
  config.vm.define "#{VM_ID}-master" do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "#{VM_ID}-master"
      vb.memory = 8192
      vb.cpus = 2
    end
    node.vm.hostname = "#{VM_ID}-master"
    node.vm.network :private_network, ip: "192.168.11.10"
  end

# Worker nodes
  (1..NUM_WORKER_NODES).each do |i|
    config.vm.define "#{VM_ID}-worker-#{i}" do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.name = "#{VM_ID}-worker-#{i}"
        vb.memory = 4096
        vb.cpus = 2
      end
      node.vm.hostname = "#{VM_ID}-worker-#{i}"
      node.vm.network :private_network, ip: "192.168.11.1#{i}"
    end
  end
  config.vm.provision "shell",
    run: "always",
    inline: "apt-get install net-tools && route add default gw 192.168.1.2"
end
```

Spin up

```bash
vagrant up
```

Check status

```bash
vagrant status
```

Access to server

```bash
vagrant ssh <vagrant_machine_name>
```

Shutdown

```bash
vagrant halt
```

Cleanup

```bash
vagrant destroy -f
```
