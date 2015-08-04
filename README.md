# ARM Toolchain for Vagrant

Vagrantfile to create a Linux virtual machine with a full GCC ARM toolchain for compiling ARM code and flashing it with tools like OpenOCD &amp; STLink.  Using this virtual machine you can get setup to compile and load ARM code
from any platform that Vagrant supports (Windows, Mac OSX, Linux).

The ARM toolchain that will be setup includes the following tools:

*   [GCC ARM Embedded](https://launchpad.net/gcc-arm-embedded) (version 4.8 2014 q1) - Full GCC toolchain
    for compiling code to run on ARM processors.
*   [OpenOCD](http://openocd.org/) (version 0.9.0) - Opensource on-chip-debugging tool.
*   [stlink command line tools](https://github.com/texane/stlink) (version 1.1.0) - Tools to program and debug 
    STM32 processors using an STLink programmer.

In addition the virtual machine will be setup to automatically passthrough a [STLink V2 programmer](https://www.adafruit.com/products/2548)
for easy flashing of STM32 microprocessors.

## Requirements

Make sure you have the latest version of [Vagrant](https://www.vagrantup.com/downloads.html) and
[VirtualBox 5.x](https://www.virtualbox.org/wiki/Downloads) installed.

In addition you will need the VirtualBox extension pack to provide USB device passthrough support.  [Download the appropriate extension pack](https://www.virtualbox.org/wiki/Downloads)
for your version of VirtualBox.  On Windows double click the downloaded .vbox-extpack file to install.  On Linux or Mac OSX install it using the VBoxManage command by navigating to the location of the
downloaded file and running:

    VBoxManage extpack install <ext pack filename>

For example if the file is called Oracle_VM_VirtualBox_Extension_Pack-5.0.0-101573.vbox-extpack then run:

    VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.0.0-101573.vbox-extpack

Finally if you are running a Linux operating system you will want to add your user to the `vboxusers` group
so that the virtual machine can access your USB devices.  Run the following command:

    sudo usermod -a -G vboxusers $USER

Then **log out and log back in** to make sure the group change takes effect.

## Usage

To start the VM open a terminal in the directory with the Vagrantfile and run:

    vagrant up

The first time the VM is started it will download an operating system image and provision it with the ARM
toolchain.  This process can take ~5-30 minutes depending on the speed of your internet connection and computer.
After the initial provisioning future VM startups will take just a few seconds.

Once the VM is running you can connect to it by running:

    vagrant ssh

You will now be inside the VM and ready to run commands that use the ARM toolchain.

When you're ready to stop the VM you can exit it by running the following command inside the VM:

    exit

Note that after you exit the VM it will still be running!  You can enter the VM again with the ssh command,
or you can shutdown the VM by running:

    vagrant halt

If you would ever like to completely delete the VM and start fresh you can remove it with:

    vagrant destroy

To transfer files between the virtual machine and your real machine you can use [Vagrant's synced folders](http://docs.vagrantup.com/v2/synced-folders/index.html).
Inside the virtual machine the `/vagrant` path will be syncronized with the location of the Vagrantfile on
your real machine.  You can copy files to and from these locations to move data between the two machines.

To pass through USB devices from your real machine to the virtual machine, consult the [VirtualBox USB documentation](https://www.virtualbox.org/manual/ch03.html#idp96037808).

For more information [consult the Vagrant documentation](https://docs.vagrantup.com/v2/).
