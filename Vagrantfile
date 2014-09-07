require 'yaml'

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Load local configuration
  local_config = YAML.load_file('./config.yml')

  # Enable plugins
  config.berkshelf.enabled = true
  config.omnibus.chef_version = :latest

  # Define a virtual machine in a block. There could be numerous machines defined in here, just copy&paste if you need
  # and specify the configuration name in every call to vagrant!
  config.vm.define "vmname" do |machine|

    # This is where you define your local virtualbox instance
    machine.vm.box = "ubuntu/trusty64"
    machine.vm.hostname = "vmname"
    machine.vm.network :private_network, ip: "33.33.33.200"
    machine.vm.network :forwarded_port, host: 9443, guest: 443
    machine.vm.network :forwarded_port, host: 9080, guest: 80

    # This is the main place to configure the provisioning
    machine.vm.provision :chef_solo do |chef|
      chef.node_name = 'vmname'
      chef.add_recipe "git"
    end

    # Customization for the virtualbox provider
    machine.vm.provider :virtualbox do |v|
      v.name = "vmname"
      v.customize ["modifyvm", :id, "--memory", "512"]
      v.customize ["modifyvm", :id, "--cpus", "1"]
    end

    # Customization for the digital_ocean provider
    machine.vm.provider :digital_ocean do |v, override|
      override.vm.box = 'digital_ocean'
      override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"

      override.ssh.private_key_path = local_config['ssh_private_key_path']
      v.token = local_config['digital_ocean_api_token']
      v.ssh_key_name = local_config['ssh_key_name']

      v.image = 'Ubuntu 14.04 x64'
      v.region = 'ams2'
      v.size = '512mb' # !!!This directly affects your DigitalOcean bill!!!
    end
  end
end
