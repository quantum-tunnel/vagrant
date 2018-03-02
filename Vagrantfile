Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.define 'mgmt' do |mgmt|
    mgmt.vm.provider "virtualbox" do |vb|
      vb.name = 'quantum-tunnel-mgmt'
    end
    mgmt.vm.network "private_network", ip: "192.0.2.20"
    mgmt.vm.hostname = 'quantum-tunnel-mgmt'
    mgmt.vm.provision :ansible_local do |ansible|
      ansible.playbook     = "ansible/servers.yml"
      ansible.extra_vars   = { tunnel_target: "198.19.0.0", tunnel_device_ip: "192.0.2.10", tunnel_device_nic: "enp0s8" }
      ansible.install_mode = "pip_args_only"
      ansible.pip_args     = "--upgrade ansible netaddr"
      ansible.become       = true
    end
  end
  config.vm.define 'local' do |local|
    local.vm.provider "virtualbox" do |vb|
      vb.name = 'quantum-tunnel-local'
    end
    local.vm.network "private_network", ip: "192.0.2.10"    #enp0s8 local
    local.vm.network "private_network", ip: "198.51.100.10" #enp0s9 transit
    local.vm.hostname = 'quantum-tunnel-local'
    local.vm.provision :ansible_local do |ansible|
      ansible.playbook     = "ansible/local.yml"
      ansible.install_mode = "pip_args_only"
      ansible.pip_args     = "--upgrade ansible netaddr"
      ansible.become       = true
    end
  end
  config.vm.define 'remote' do |remote|
    remote.vm.provider "virtualbox" do |vb|
      vb.name = 'quantum-tunnel-remote'
    end
    remote.vm.network "private_network", ip: "203.0.113.10"  #enp0s8 remote
    remote.vm.network "private_network", ip: "198.51.100.20" #enp0s9 transit
    remote.vm.hostname = 'quantum-tunnel-remote'
    remote.vm.provision :ansible_local do |ansible|
      ansible.playbook     = "ansible/remote.yml"
      ansible.install_mode = "pip_args_only"
      ansible.pip_args     = "--upgrade ansible netaddr"
      ansible.become       = true
    end
  end
  config.vm.define 'target' do |target|
    target.vm.provider "virtualbox" do |vb|
      vb.name = 'quantum-tunnel-target'
    end
    target.vm.network "private_network", ip: "203.0.113.20"
    target.vm.hostname = 'quantum-tunnel-target'
    target.vm.provision :ansible_local do |ansible|
      ansible.playbook     = "ansible/servers.yml"
      ansible.extra_vars   = { tunnel_target: "198.18.0.0", tunnel_device_ip: "203.0.113.10", tunnel_device_nic: "enp0s8" }
      ansible.install_mode = "pip_args_only"
      ansible.pip_args     = "--upgrade ansible netaddr"
      ansible.become       = true
    end
  end
end
