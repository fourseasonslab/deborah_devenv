# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.25.31"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM
  	vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # apt-get update
    sudo apt-get install -y build-essentials
	sudo apt-get install -y g++
	# mecab
	if [ ! -d ./mecab-0.996 ]; then
	  cp /vagrant/package/mecab-0.996.tar.gz ./
	  tar xvf mecab-0.996.tar.gz
	fi
	if [ ! -f /usr/local/bin/mecab ]; then
		cd mecab-0.996
			./configure
			make
			sudo make install
		cd ..
	fi
	# ipadic
	if [ ! -d ./mecab-ipadic-2.7.0-20070801 ]; then
	  cp /vagrant/package/mecab-ipadic-2.7.0-20070801.tar.gz  ./
	  tar xvf mecab-ipadic-2.7.0-20070801.tar.gz
	fi
	cd mecab-ipadic-2.7.0-20070801
		./configure --with-charset=utf8
		make
		sudo make install
	cd ..
	# crf++
	if [ ! -d ./CRF++-0.58 ]; then
	  cp /vagrant/package/CRF++-0.58.tar.gz ./
	  tar xvf CRF++-0.58.tar.gz
	fi
	if [ ! -f /usr/local/lib/libcrfpp.a ]; then
		cd CRF++-0.58
			./configure
			make
			sudo make install
		cd ..
	fi
	# libpath
	sudo bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/libpath_for_cabocha.conf'
	sudo ldconfig
	# cabocha
	if [ ! -d ./cabocha-0.69 ]; then
	  cp /vagrant/package/cabocha-0.69.tar.bz2 ./
	  tar xvf cabocha-0.69.tar.bz2
	fi
	cd cabocha-0.69
		make clean
		./configure --with-charset=utf8
		make
		sudo make install
	cd ..
	#
	sudo ldconfig
  SHELL
end
