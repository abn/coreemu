# Linux Network Namespaces #

## Network Namespace Supported Features ##

Even with the latest Linux kernels, not all networking features may be ready for use with network namespaces.  Generally, the newer the kernel, the better the support for netns. Here are notes organized by feature.

  * IPsec - 3.2.0 works OK<br>for older kernels, patch available, see below.<br>
<ul><li>netfilter queuing - patch available, see below.<br>
</li><li>interface index - patch available, see below.<br>
</li><li>Linux bridging - works on Linux 3.0.0, but not on some earlier  kernels<br>e.g.: 2.6.32-220.4.1.el6.x86_64 kernel panics with bridging<br>for 3.0.0, note that <b>brctl show</b> only shows bridges on the host, not within a netns<br>for 3.2.0-26-generic, brctl works fine on Ubuntu 12.04<br>
</li><li>multicast routing - XORP PIM will run under Ubuntu 12.04 3.2.0-57-generic x86_64 kernel<br>
</li><li>QoS with tc queuing - TBD<br>
</li><li>GRE tunneling - TBD</li></ul>


<h2>CORE kernel patches</h2>

Some kernel patches are available from the CORE team, check a source tarball under the <code>kernels/</code> directory.<br>
<br>
For example, CORE 4.3 includes a 2.6.38 kernel patch that addresses the following:<br>
<ul><li>virtualize the netfilter queue mechanism (allows iptables NFQUEUE targets)<br>
</li><li>disable the flow cache (allows using IPsec between namespaces)<br>
</li><li>virtualize network interface index numbers</li></ul>

See the <code>README.txt</code> file that comes with the kernel patches. A <code>Makefile</code> may be included for easily building a deb kernel package.<br>
<br>
<br>
<h2>Running a custom kernel PPA on Ubuntu</h2>

PPA = Personal Package Archive, an unsupported/bleeding-edge repository of deb packages.<br>
<br>
The example here is installing the 2.6.35-rc4 kernel on Ubuntu 10.04. Normally 10.04 (Lucid) gives you a 2.6.32 kernel, but the 2.6.35 kernel features improved namespace support for netlink and iptables (thanks to patches submitted by Tom Goff.) This will allow us to run SMF for example, using netlink queueing to capture packets, and allows using the <code>tc</code> command for applying QoS rules.<br>
<br>
<ol><li>add the <a href='https://launchpad.net/~kernel-ppa/+archive/ppa'>PPA for Ubuntu Kernel</a> to your system. Follow the link for more instructions.<br>
<pre><code>sudo add-apt-repository ppa:kernel-ppa/ppa<br>
</code></pre>
<blockquote>If you are behind a firewall/proxy, the above command will likely fail to import the key from the keyserver. Solution can be found here <a href='http://www.linuxquestions.org/questions/linux-software-2/adding-ppa-key-to-apt-748592/'>http://www.linuxquestions.org/questions/linux-software-2/adding-ppa-key-to-apt-748592/</a>
</blockquote></li><li>install the new kernel<br>
<pre><code># on 2010-07-07 this installed linux-image-2.6.35-7-generic<br>
sudo apt-get install linux-image-generic-lts-backport-maverick<br>
</code></pre>
</li><li>reboot to enable the new kernel; with the new GRUB 2, you need to hold down SHIFT if you want to interrupt startup and select a kernel.</li></ol>

<h2>Building a custom Ubuntu kernel</h2>
Follow the directions here:<br>
<ul><li><a href='https://wiki.ubuntu.com/KernelTeam/GitKernelBuild'>https://wiki.ubuntu.com/KernelTeam/GitKernelBuild</a>
</li><li>when running <code>make oldconfig</code> watch out for "CONFIG_X86_MRST=y" on 2.6.35 and set it to "n", or keyboard/mouse may be disabled (<a href='http://ubuntuforums.org/showthread.php?p=9562261'>thread</a>)</li></ul>

<h1>FreeBSD</h1>

A small patch is required to the standard FreeBSD 8.x kernel for running CORE. That patch is included in the CORE source under the <code>kernel/</code> directory. See the FreeBSD installation instructions for recompiling the kernel.<br>
<br>
This patch applies cleanly to the FreeBSD 9.0-RELEASE generic kernel.