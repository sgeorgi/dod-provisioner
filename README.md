# DigitalOcean Droplet Provisioner

This is a Vagrant/Berkshelf repository to create, manage and provision Droplets on DigitalOcean by using Chef-Solo,
accompanyied by cookbooks provisioned by Berkshelf.

The main use case for this is to create and provision virtual machines locally - by using the :virtualbox provider - 
and then create Droplets based on the locally proven Chef-Solo provisioning. 

The provided Vagrantfile included a configuration named :vmname that is being provisioned by Chef-Solo with a yet
to be defined combination of recipes, roles and run-lists. The exact same provisioning can be used to create a
Droplet on DigitalOcean.

Chef cookbooks are mainly provided by using Berkshelf, which lets you manage dependencies and required cookbooks in the
Berksfile. It automatically stages any downloaded cookbook locally and either mounts them to your VirtualBox instance or
rsyncs them to your Droplet.

Simply do a `vagrant up --provider=digital_ocean` to create a machine remotely, leave out the --provider option to do
everything locally.

Feel free to fork, modify or copy/reuse to you own purposes!

### Prerequisites

First of all you have to install the neccesary vagrant plugins:
    
    vagrant plugin install vagrant-omnibus
    vagrant plugin install vagrant-berkshelf
    vagrant plugin install vagrant-digitalocean
    
Copy over config.yml.sample to config.yml and provide the neccesary values. The SSH-key identifier should be what
DigitalOcean offers you as an SSH key while creating a Droplet manually, the corresponding private key needs to be
somewhere on your local machine!

### Provisioning

Configuring your local VM should be pretty straight-forward if you know Vagrant and Chef. Add any provisioning steps in the 

    machine.vm.provision :chef_solo do |chef|
      chef.add_recipe "git"
    end
    
block. You can also reference recipes or run_lists of your own local cookbook/chef file structure inside the local directory!

## TO-DO

 * This project is discontinued as I focus on setting Bosh + CloudFoundy on my own hardware in the future. 

## WARNING

Any droplets created will basically cost you money, so be sure to either plan to spend money or to issue a `vagrant destroy` 
