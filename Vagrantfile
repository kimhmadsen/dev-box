# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# packages to be added to the VM.
$script = <<SCRIPT
echo I am provisioning...
apt-get update
apt-get install tmux vim subversion git git-svn ruby2.0 -y
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # vb.name = "udvbuildserver"
    vb.memory = 1024
    vb.cpus = 4
    vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
  end
  config.vm.provision :shell, :inline => $script
  #config.vm.provision :shell, :path => "bootstrap.sh"
    
end

