Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "public_network"
  config.vm.define 'local' do |local|
    local.vm.provider "virtualbox" do |vb|
      vb.name = 'quantum-tunnel-local'
    end
    local.vm.network "private_network", ip: "10.8.8.5"
    local.vm.hostname = 'quantum-tunnel-local'
    local.vm.provision :ansible_local do |ansible|
      ansible.playbook = "ansible/local.yml"
      ansible.install  = true
      ansible.become   = true
    end
  end
  config.vm.define 'remote' do |remote|
    remote.vm.provider "virtualbox" do |vb|
      vb.name = 'quantum-tunnel-remote'
    end
    remote.vm.network "private_network", ip: "10.8.8.6"
    remote.vm.hostname = 'quantum-tunnel-remote'
    remote.vm.provision :ansible_local do |ansible|
      ansible.playbook = "ansible/remote.yml"
      ansible.install  = true
      ansible.become   = true
    end
  end
end

