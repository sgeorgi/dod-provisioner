VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.berkshelf.enabled = true

  config.vm.define :test do |client|
      client.vm.box = "ubuntu/trusty64"
      client.vm.hostname = "test"
      client.vm.network :private_network, ip: "33.33.33.200"
      client.vm.network :forwarded_port, host: 9443, guest: 443
      client.vm.network :forwarded_port, host: 9080, guest: 80

      client.vm.provision :chef_solo do |chef|
        chef.node_name = 'test_node'
        chef.add_recipe "nginx"
      end

      client.vm.provider "virtualbox" do |v|
        v.name = "test"
        v.customize ["modifyvm", :id, "--memory", "512"]
        v.customize ["modifyvm", :id, "--cpus", "1"]
      end
    end
end