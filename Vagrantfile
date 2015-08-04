# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
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
  # config.vm.network "private_network", ip: "192.168.33.10"

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
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Turn on USB 2.0 support and add usb filter to attach STLink V2 programmer.
  # Note that the VirtualBox extension pack MUST be installed first from:
  #   https://www.virtualbox.org/wiki/Downloads
  # Also on Linux be sure to add your user to the vboxusers group, see:
  #   http://unix.stackexchange.com/questions/129305/how-can-i-enable-access-to-usb-devices-within-virtualbox-guests
   config.vm.provider :virtualbox do |vb|
     vb.customize ['modifyvm', :id, '--usb', 'on']
     vb.customize ['modifyvm', :id, '--usbehci', 'on']
     vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'STLink', '--vendorid', '0x0483', '--productid', '0x3748']
   end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
     sudo dpkg --add-architecture i386
     sudo apt-get update
     sudo apt-get install -y build-essential autotools-dev autoconf pkg-config libusb-1.0-0 libusb-1.0-0-dev libftdi1 libftdi-dev git libc6:i386 libncurses5:i386 libstdc++6:i386
     cd /home/vagrant
     wget http://downloads.sourceforge.net/project/openocd/openocd/0.9.0/openocd-0.9.0.tar.gz
     tar xvfz openocd-0.9.0.tar.gz
     cd openocd-0.9.0
     ./configure
     make
     sudo make install
     cd /home/vagrant
     wget https://github.com/texane/stlink/archive/1.1.0.tar.gz
     mv 1.1.0.tar.gz stlink-1.1.0.tar.gz
     tar xvfz stlink-1.1.0.tar.gz
     cd stlink-1.1.0
     ./autogen.sh
     ./configure
     make
     sudo make install
     sudo cp 49-stlink*.rules /etc/udev/rules.d/
     sudo udevadm control --reload-rules
     sudo udevadm trigger
     cd /home/vagrant
     wget https://launchpad.net/gcc-arm-embedded/4.8/4.8-2014-q1-update/+download/gcc-arm-none-eabi-4_8-2014q1-20140314-linux.tar.bz2
     tar xjf gcc-arm-none-eabi-4_8-2014q1-20140314-linux.tar.bz2
     sudo mv gcc-arm-none-eabi-4_8-2014q1 /usr/local/
     echo 'export PATH=/usr/local/gcc-arm-none-eabi-4_8-2014q1/bin:$PATH' >> /home/vagrant/.bashrc
     export PATH=/usr/local/gcc-arm-none-eabi-4_8-2014q1/bin:$PATH
  SHELL
end
