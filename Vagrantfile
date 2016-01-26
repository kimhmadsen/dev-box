# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# packages to be added to the VM.
$script = <<SCRIPT
echo I am provisioning...
apt-get update
apt-get install tmux vim subversion git git-svn ruby2.0 ruby-bundler pry -y
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.synced_folder "..", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    # Don't boot with headless mode
#    vb.gui = true
  
#    # Use VBoxManage to customize the VM. For example to change memory:
#    vb.name = "udvbuildserver"
    vb.memory = 1024
    vb.cpus = 4
    vb.linked_clone = true if Vagrant::VERSION =~ /^1.8/
    vb.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
  end
  config.vm.provision :shell, :inline => $script
  #config.vm.provision :shell, :path => "bootstrap.sh"

  if File.exist?(Dir.home + '/.gitconfig')
  config.vm.provision( :file,
    :source      => '~/.gitconfig',
    :destination => '/home/vagrant/.gitconfig' )
  end

  Dir[Dir.home + "/.ssh/*"].each do |path|
    dest_path = '/home/vagrant/.ssh/' + File.basename(path)
    if File.basename(path) != "authorized_keys"
      config.vm.provision :file do |file|
        file.source      = path
        file.destination = dest_path
      end
      if File.extname(path) == ".key" or File.basename(path) =~ /id_.*/
        config.vm.provision :shell, :inline => "chmod 600 #{dest_path}"
      end
    end
  end

  subversion_home_dir = Dir.home + "/.subversion"
  subversion_appdata_dir = Dir.home + "/AppData/Roaming/Subversion"
  subversion_dir =  Dir[subversion_home_dir, subversion_appdata_dir][0]
  if subversion_dir != nil
    Dir[subversion_dir + "/**/*"].each do |path|
      if not File.directory? path
        dest_path = path.sub( subversion_dir, '/home/vagrant/.subversion/')
        config.vm.provision :file do |file|
          file.source      = path
          file.destination = dest_path
        end
      end
    end
  end

end

