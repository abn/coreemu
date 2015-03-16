This page is dedicated to tips for VCORE, the CORE virtual machine. Currently VCORE is provided using a VMware vmx and vmdk format because it seems to have the best support from virtualization tools (VMware Server, VMware Player, VirtualBox, Parallels, etc.). Using the OVF format would be nice but experience has shown limited and broken support from these tools.

We recommend using [VirtualBox](http://www.virtualbox.org) for running VCORE.

# Disclaimer #
Please heed the disclaimer posted on the [Quickstart](Quickstart.md) page and in the [CORE manual](http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html#vcore). The virtual machine is really for evaluation purposes and not for serious work. You should really set up a Ubuntu or Fedora Linux machine. It's not too difficult.

_Note that you may experience decreased performance and timer issues, and there is the added confusion of virtualization within virtualization. If you decide to run in this mode, we suggest using SSH with X11 forwarding into the VCORE virtual machine, to mitigate the decreased performance of the VMware virtual display._

# Importing VCORE into VirtualBox #

Unzip `vcore-4.3.zip` to your virtual machines directory; a `vcore-4.3` folder will be created.

Start the VirtualBox GUI, and from the _Machine_ menu, choose _Add_. Browse to the folder where you unzipped VCORE, and choose the `vcore-4.3.vbox` file.

# Importing VCORE into VMware #

Unzip `vcore-4.3.zip` to your virtual machines directory; a `vcore-4.3` folder will be created. On Windows this may be under your Documents folder under `My Virtual Machines`.

If you already have VMware installed, double-click on the vmx file to open the VCORE virtual machine with VMware.

Ubuntu has an `open-vm-tools` package to provide VMware Player compatibility. You can install it with:
```
sudo apt-get install open-vm-tools open-vm-toolbox
```


# VCORE usage notes #

Note the RAM and number of processors. If you have a quad-core system with hyperthreading (8 threads), then you may want to assign up to 4 processors to VCORE. 2GB is a good amount of RAM for VCORE; unless your machine only has 2 or 3GB of physical RAM, then you may try 1GB for VCORE.

The machine should boot to the Ubuntu desktop when powered on. The username and password can be found in the included README file. Ubuntu does not have a root password, but instead uses `sudo`.