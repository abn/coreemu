# Quickstart: Installation using pre-built packages #

This is the quickstart guide; for full installation instructions, refer to the [CORE manual](http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html).

CORE 4.7 packages are available for Ubuntu 12.04 (precise) or 14.04 (trusty), Fedora 19 or 20, and rhel7 or rhel6. Ubuntu (14.04) and Debian users can install CORE directly using `sudo apt-get install core-network`. For different Linux distributions, make sure the kernel is at least 2.6.27 to support [network namespaces](Namespaces.md). Released packages are generally more stable and tested to work properly on the given platform. See the [upgrading](Upgrading.md) page for help choosing a release or for upgrade instructions.

## Step 1. Install CORE package ##

Get the Ubuntu (deb) or Fedora (rpm) package for your architecture (64-bit or 32-bit) from here:<br>
<a href='https://code.google.com/p/coreemu/downloads/list'>https://code.google.com/p/coreemu/downloads/list</a>

Use your package manager to automatically install dependencies:<br>
<pre><code>  # Ubuntu 14.04 users - from repo<br>
  sudo apt-get install core-network<br>
  # Ubuntu users - from deb files<br>
  software-center core-daemon_4.7-0ubuntu1_precise_amd64.deb<br>
  software-center core-gui_4.7-0ubuntu1_precise_all.deb<br>
  # Fedora users - from RPM files<br>
  su<br>
  yum localinstall core-daemon-4.7-1.fc20.x86_64.rpm<br>
  yum localinstall core-gui-4.7-1.fc20.noarch.rpm<br>
</code></pre>

Virtual networks usually require routing software. CORE automatically builds configs for the <a href='http://www.quagga.net'>Quagga</a> routing suite. For wireless networks, CORE will generate OSPF MDR configs, for a modified version of Quagga available from <a href='http://www.nrl.navy.mil/itd/ncs/products/ospf-manet'>here</a>.<br>
<pre><code>  # Ubuntu - vanilla Quagga<br>
  sudo apt-get install quagga<br>
  # Ubuntu - OSPF MDR Quagga<br>
  software-center /home/user/Downloads/quagga-mr_0.99.21mr2.2_amd64.deb<br>
  # Fedora - vanilla Quagga currently unsupported (see ReleaseNotes)<br>
  #su<br>
  #yum install quagga<br>
  # Fedora - OSPF MDR Quagga<br>
  su<br>
  yum localinstall quagga-0.99.21mr2.2-1.fc16.x86_64.rpm --nogpgcheck<br>
</code></pre>

Check the <b>Known Issues</b> section in the ReleaseNotes.<br>
<br>
<b>Fedora users:</b> check the full <a href='http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html#installing-from-packages-on-fedora-centos'>installation instructions</a>, you need to also disable (or configure) SELINUX, sandbox, and iptables.<br>
<br>
<b>CentOS users:</b> check the full <a href='http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html#installing-from-packages-on-fedora-centos'>installation instructions</a> for enabling the EPEL repo.<br>
<br>
<br>
To check the RPM signing key, the core-dev public key can be found here: <a href='https://coreemu.googlecode.com/files/RPM-GPG-KEY-core-dev'>RPM-GPG-KEY-core-dev</a>

<h2>Step 2. Start the CORE daemon</h2>
<pre><code>  # for Ubuntu/CentOS<br>
  sudo /etc/init.d/core-daemon start<br>
  # for Fedora<br>
  su<br>
  systemctl start core-daemon.service<br>
</code></pre>

To view the CORE daemon log file, look at <code>/var/log/core-daemon.log</code>.<br>
<br>
<h2>Step 3. Run CORE</h2>

This is typically run as a normal (non-root) user.<br>
<pre><code>  core-gui<br>
</code></pre>

You'll see warnings about missing conf files the first time you run the CORE GUI.<br>
<br>
The sample topology <code>~/.core/configs/sample1.imn</code> will test that CORE and OSPF MDR Quagga are correctly installed.<br>
<br>
<h1>Quickstart: Installation from source</h1>
Follow these instructions to build CORE from source. Dependencies are automatically resolved when you use your package manager with the CORE packages shown above, but not when compiling from source. The source used could be a released version (see above) or the bleeding-edge <a href='http://downloads.pf.itd.nrl.navy.mil/core/source/nightly_snapshots/'>nightly SVN snapshot</a> or <a href='https://code.google.com/p/coreemu/source/checkout'>subversion</a>

<h2>Step 1. Install prerequisite packages</h2>
Ubuntu:<br>
<br>
<code>sudo apt-get install bash bridge-utils ebtables iproute libtk-img python tcl8.5 tk8.5 autoconf automake gcc libev-dev make pkg-config python-dev libreadline-dev imagemagick help2man</code>

Fedora:<br>
<br>
<code>yum install bash bridge-utils ebtables iproute libev python tcl tk tkimg autoconf automake gcc libev-devel make pkgconfig python-devel ImageMagick help2man</code>

This package list comes from<br>
<a href='http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html'>here</a>.<br>
<br>
You should also install Quagga for routing (<code>sudo apt-get install quagga</code>  or <code>yum install quagga</code>), or some other routing package. For wireless MANET networks, CORE can build configs for <a href='http://www.nrl.navy.mil/itd/ncs/products/ospf-manet'>OSPF MDR</a>.<br>
<br>
<b>Fedora users:</b> check the full <a href='http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html#installing-from-packages-on-fedora-centos'>installation instructions</a>, you need to also disable (or configure) SELINUX, sandbox, and iptables.<br>
<br>
<br>
<h2>Step 2. Get and build the source</h2>
You can get the latest SVN snapshot or a release version at the<br>
<a href='http://downloads.pf.itd.nrl.navy.mil/core/source/'>downloads page</a>.<br>
<br>
In this example, we download the SVN source snapshot <a href='http://downloads.pf.itd.nrl.navy.mil/core/source/nightly_snapshots/core-svnsnap.tgz'>core-svnsnap.tgz</a>

<pre><code>  tar xzf core-svnsnap.tgz<br>
  cd core<br>
  ./bootstrap.sh<br>
  # on Fedora use ./configure --with-startup=systemd<br>
  ./configure<br>
  # -j8 is for 8 concurrent jobs on a multi-core system<br>
  make -j8<br>
  sudo make install<br>
</code></pre>

<h2>Step 3. Start the CORE daemon</h2>
<pre><code>sudo core-daemon<br>
</code></pre>

For verbose debugging:<br>
<pre><code>sudo core-daemon -v<br>
</code></pre>

Using the init script (messages go to <code>/var/log/core-daemon.log</code>):<br>
<pre><code>sudo /etc/init.d/core-daemon start<br>
</code></pre>

<h2>Step 4. Run the CORE GUI</h2>
This is typically run as a normal (non-root) user.<br>
<pre><code>core-gui<br>
</code></pre>

<h1>Tips</h1>
<ul><li>Once you see the CORE GUI, you can try loading one of the sample configuration such as <code>sample1.imn</code>
</li><li>Check for notes on your Linux distribution on the <a href='Linux.md'>Linux</a> and <a href='ReleaseNotes.md'>ReleaseNotes</a> pages.<br>
</li><li>CORE requires the Tcl/Tk packages, version 8.5. You may see warnings about specific package versions on FreeBSD, these may be safely ignored.<br>
</li><li>For system requirements and tips on the install process, visit the <a href='http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html#prerequisites'>Prerequisites</a> page of the <a href='http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/'>CORE manual</a></li></ul>

<h1>Other Options</h1>
<ul><li><b><a href='EMANE.md'>EMANE</a> install</b></li></ul>

<ul><li><b><a href='SDT3D.md'>SDT3D</a> install</b></li></ul>

<ul><li><b>Do I need a custom kernel?</b> Not usually. Some disro's stock kernels may have issues, see <a href='Linux.md'>Linux</a> and <a href='NamespaceKernels.md'>NamespaceKernels</a>. Some specific networking stack features may not support network namespaces, see <a href='CustomKernels.md'>CustomKernels</a>.</li></ul>

<ul><li><b>VMware image</b><br><b>Pros:</b> everything is already set up for you; one file to download.  <b>Cons:</b> poor performance; for quick evaluation only.<br><a href='http://downloads.pf.itd.nrl.navy.mil/core/vmware-image/'>http://downloads.pf.itd.nrl.navy.mil/core/vmware-image/</a><br>You can run CORE from within this pre-built VMware virtual machine with minimal setup. See the included vcore-<code>*</code>-README.txt in the zipfile for login information.  <i>Note that you may experience decreased performance and timer issues, and there is the added confusion of virtualization within virtualization. If you decide to run in this mode, we suggest using SSH with X11 forwarding into the VCORE virtual machine, to mitigate the decreased performance of the virtual display.</i>