# Distribution Notes #

## Debian-based ##
### Ubuntu 10.04 ###
  * Some kernel versions do not have namespace support turned on. With 2.6.32-32 kernel, you may get the vnoded error: `nsfork() failed: Invalid argument`
### Ubuntu 10.10 ###
### Ubuntu 11.04 ###
### Ubuntu 11.10 ###

## Redhat-based ##
### Fedora 13 ###
### Fedora 14 ###
### Fedora 15 ###
  * Set `SELINUX=permissive` in `/etc/sysconfig/selinux`. If you set `SELINUX=disabled` you'll also want to add `selinux=0` to your kernel line in `/etc/grub.conf`.
  * Turn off SELinux sandbox using `chkconfig sandbox off` and rebooting. Sandbox interferes with CORE's bind mounts, and all nodes will appear to have the same mounts (regardless of namespace). The output of "mount" will be lengthy and confusing.
  * Update to the 2.6.40.3-0.fc15.x86\_64 (or newer) kernel using yum: "yum update kernel". The 2.6.38.6-26.rc1.fc15.x86\_64 appears to have an IPv6 multicast checksum bug (see [NamespaceKernels](NamespaceKernels.md))
### Fedora 16 ###
## Other ##
### OpenSUSE 11.3 ###
### OpenSUSE 11.4 ###
### Arch Linux ###
  * Users have provided these tips:<br>Here are the packages required for Arch Linux: <code>sudo pacman -S bridge-utils ebtables, iproute, libev, tcl, tk, tkimg, python, xterm, lxc, autoconf, automake, make, libtool, readline, xauth</code><br> ebtables, lxc, and tkimg are only available through aur.archlinux.org.<br>A user-made AUR package for CORE: <a href='https://aur.archlinux.org/packages.php?ID=42250'>https://aur.archlinux.org/packages.php?ID=42250</a></li></ul>


<h1>Other Wiki Links</h1>

<h2>Linux Network Namespaces</h2>
<ul><li>Linux Network <a href='Namespaces.md'>Namespaces</a> and CORE<br>
</li><li>See known good/bad kernels in <a href='NamespaceKernels.md'>NamespaceKernels</a></li></ul>

<h2>Linux OpenVZ</h2>
<ul><li><a href='LinuxOpenVZTemplateCreation.md'>Instructions for creating your own template cache</a>
</li><li><a href='AddingApplicationsToOpenVz.md'>Adding applications to OpenVZ containers</a>
</li><li><a href='LinuxOpenVZNetworkAccess.md'>OpenVZ container access to the external network</a>
</li><li><a href='Fedora.md'>Fedora</a> OpenVZ installation tips<br>
</li><li>Note that a majority of the useful OpenVZ documentation will be available from the <a href='http://www.openvz.org'>OpenVZ</a> website.