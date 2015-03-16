# Introduction #

Linux network namespaces (netns) is a lightweight container-based virtualization that is part of the mainline 2.6.27+ Linux kernel. A virtual network stack can be associated with a process group. This is similar to the FreeBSD jail mechanism.

Each namespace has its own loopback device and process space. Virtual or real devices can be added to each network namespace, and you can assign IP addresses to these devices and use them as a network node. By default these network namespaces share the same filesystem, just like CORE nodes in FreeBSD. Netns does not have the same security and resource restrictions as OpenVZ containers, and do not require a separate OS template.

You do not need to patch your kernel in order to use network namespaces. Modern distros such as Ubuntu 10.04 and Fedora 13 have netns support turned on in their default kernels.

NamespaceKernels has a list of known good and bad kernels.
CustomKernels has notes on Network Namespace Supported Features.

# Command-line Example #

Below is an example of building two virtual nodes from the command-line, and connecting them with a bridge (a wired network in CORE.) The `vnoded` and `vcmd` commands are provided by CORE. See the Python script examples for doing this at a higher level with Python scripting.

```
#!/bin/sh
# Below is a transcript of creating two emulated nodes and connecting them 
# together with a wired link. You can run the core-cleanup.sh script to clean
# up after this script.

# create node 1 namespace container
vnoded -c /tmp/n1.ctl -l /tmp/n1.log -p /tmp/n1.pid
# create a virtual Ethernet (veth) pair, installing one end into node 1
ip link add name n1.0.1 type veth peer name n1.0
ip link set n1.0 netns `cat /tmp/n1.pid`
vcmd -c /tmp/n1.ctl -- ip link set n1.0 name eth0
vcmd -c /tmp/n1.ctl -- ifconfig eth0 10.0.0.1/24

# create node 2 namespace container
vnoded -c /tmp/n2.ctl -l /tmp/n2.log -p /tmp/n2.pid
# create a virtual Ethernet (veth) pair, installing one end into node 2
ip link add name n2.0.1 type veth peer name n2.0
ip link set n2.0 netns `cat /tmp/n2.pid`
vcmd -c /tmp/n2.ctl -- ip link set n2.0 name eth0
vcmd -c /tmp/n2.ctl -- ifconfig eth0 10.0.0.2/24

# bridge together nodes 1 and 2 using the other end of each veth pair
brctl addbr b.1.1
brctl setfd b.1.1 0
brctl addif b.1.1 n1.0.1
brctl addif b.1.1 n2.0.1
ip link set n1.0.1 up
ip link set n2.0.1 up
ip link set b.1.1 up

# display connectivity and ping from node 1 to node 2
brctl show
vcmd -c /tmp/n1.ctl -- ping 10.0.0.2
```

# Kernel Development Guidelines #

Here are some general rules for making things namespace-aware:

  * move static variables to a per-namespace structure as needed
  * pass a per-namespace context argument to functions as needed; e.g., struct net or the per-namespace structure from above
  * remove references to init\_net and find the namespace dynamically when needed (use dev\_net(), sock\_net(), etc.)
  * create and register per-namespace initializer/cleanup functions (see struct pernet\_operations and net\_generic()); do namespace-specific operations here instead of in the module init/exit functions
  * create per-namespace procfs/sysfs files

Exactly what data becomes per-namespace and what is shared has to be considered on a case-by-case basis.  Also, some of the specifics depend on the kernel version.

The GRE kernel module (ip\_gre.c) might be useful as an introductory example.

# References #

Linux Containers SourceForge project page:

  * http://lxc.sourceforge.net

IBM Linux container tools tutorial:

  * http://www.ibm.com/developerworks/linux/library/l-lxc-containers/


Other helpful sites:

  * http://en.opensuse.org/LXC
  * http://wiki.archlinux.org/index.php/Linux_Containers
  * http://nigel.mcnie.name/blog/a-five-minute-guide-to-linux-containers-for-debian
  * http://sunoano.name/ws/public_xhtml/linux_containers.html
  * http://sysadmin-cookbook.rot13.org/#lxc

Mailing lists:
  * Introduction Jan 2007: http://lwn.net/Articles/219597/
  * this message discusses frustrations OpenVZ users might have with non-Redhat host distros: http://openvz.org/pipermail/users/2010-January/003190.html
  * discussion of LXC differences vs OpenVZ and Linux-VServer: http://openvz.org/pipermail/users/2010-January/003192.html