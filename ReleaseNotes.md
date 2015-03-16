Refer to the NamespaceKernels and CustomKernels pages for info on good/bad kernels and supported networking features.

# 4.7 #
CORE 4.7 was released on 8/6/14 (svn [r565](https://code.google.com/p/coreemu/source/detail?r=565)). Packages are available for the following Linux distributions:
  * debs for Ubuntu 14.04 (trusty), 12.04 (precise)
  * RPMs for Fedora 20 (fc20), 19 (fc19)
  * RPMs for CentOS/RHEL/Scientific Linux 7.x (el7), 6.x (el6)

The high-level changes since version 4.6 are:

**EMANE**
  * support for EMANE 0.9.1
  * fix error when using Comm Effect model with loss/duplicate string values
  * enable flow control in virtual transport if enabled in the MAC model
  * fix [bug #150](https://code.google.com/p/coreemu/issues/detail?id=150) where EMANE event service/address port were not used

**GUI**
  * support Tcl/Tk 8.6 when available
  * added --(a)ddress and --(p)ort arguments to core-gui command-line
  * added File > Execute XML or Python script... option
  * added File > Execute Python script with options... menu item
  * when executing Python script from GUI, run in background thread, wait for RUNTIME state
  * enter RUNTIME state when start button pressed with empty canvas
  * added support for asymmetric link effects
  * support link delays up to 274 seconds (netem maximum)
  * allow runtime changes of WLAN link effects

**DAEMON**
  * set NODE\_NAME, NODE\_NUMBER, SESSION\_SHORT in default vnoded environment
  * changed host device naming to use veth, tap prefixes; b.n.SS for bridges
  * allow parsing XML files into live running session
  * enable link effects between hub/switch and hub/switch connections
  * update MDR service to use broadcast interfaces for non-WLAN links
  * allow node class to be specified when initializing XML parser
  * save and parse canvas origin (reference point) and scale in MP XML
  * up/down control script session option
  * fix hash calculation used to determine GRE tunnel keys
  * use shell script to detach SMF on startup
  * added NRL services for mgen sink and nrlolsrv2
  * use SDT URL session option
  * added core-manage tool for addons to add/remove/check services, models, and custom node types

**API**
  * implement local flag in Execute Message for running host commands
  * jitter changed to 64-bit value to align with delay in Link Message
  * added unidirectional link flag TLV to Link Message
  * added reconfigure event type for re-generating service config files
  * return errors in API with failed services

**BUGFIXES**
  * fix HTTP service running under Ubuntu
  * fixed the following bugs: [bug #150](https://code.google.com/p/coreemu/issues/detail?id=150), [bug #169](https://code.google.com/p/coreemu/issues/detail?id=169), [bug #188](https://code.google.com/p/coreemu/issues/detail?id=188), [bug #220](https://code.google.com/p/coreemu/issues/detail?id=220), [bug #225](https://code.google.com/p/coreemu/issues/detail?id=225), [bug #230](https://code.google.com/p/coreemu/issues/detail?id=230), [bug #231](https://code.google.com/p/coreemu/issues/detail?id=231), [bug #242](https://code.google.com/p/coreemu/issues/detail?id=242), [bug #244](https://code.google.com/p/coreemu/issues/detail?id=244), [bug #247](https://code.google.com/p/coreemu/issues/detail?id=247), [bug #248](https://code.google.com/p/coreemu/issues/detail?id=248), [bug #250](https://code.google.com/p/coreemu/issues/detail?id=250), [bug #251](https://code.google.com/p/coreemu/issues/detail?id=251)

**Known Issues with 4.7**
  * Bandwidth plots and Throughput Widget broken in CORE 4.7. See [bug #257](https://code.google.com/p/coreemu/issues/detail?id=257). You can apply the one-line fix using:
```
sudo sed -i 's/set ifname "n\$node_num\\\\.\$ifname/set ifname "veth\$node_num\\\\.[string range \$ifname 3 end]/' /usr/lib/core/widget.tcl
```
  * Under Fedora or CentOS Linux, the repository version of Quagga (`yum install quagga`) does not work with CORE. See [bug #255](https://code.google.com/p/coreemu/issues/detail?id=255). You need to build Quagga or Quagga MDR from source using the [manual instructions](http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/install.html#compiling-quagga-for-core) or use the [fc16 Quagga MDR package](http://downloads.pf.itd.nrl.navy.mil/ospf-manet/) (works with fc20).
  * Under Fedora or CentOS Linux, NetworkManager will automatically invoke DHCP clients on CORE-generated interfaces. See [bug #253](https://code.google.com/p/coreemu/issues/detail?id=253). Ubuntu ignores interfaces having the "veth" prefix. Recommended workaround is to turn off NetworkManager using:
```
systemctl stop NetworkManager
systemctl disable NetworkManager
```



---

# 4.6 #
CORE 4.6 was released on 9/25/13. Packages are available for the following Linux distributions:
  * debs for Ubuntu 13.04 (raring), 12.10 (quantal), **12.04 (precise)**
  * RPMs for Fedora 19 (fc19), 18 (fc18), 17 (fc17)
  * RPMs for CentOS/RHEL/Scientific Linux 6.x (el6)

The high-level changes since version 4.5 are:

**NOTE:** `cored` is now `core-daemon` and `core` is now `core-gui` (for Debian acceptance)

**NOTE:** /etc/init.d/core is now /etc/init.d/core-daemon (for insserv compatibility)

**EMANE**
  * don't start EMANE locally if no local NEMs
  * EMANE poststartup() to re-transmit location events during initialization
  * added debug port to EMANE options
  * added a basic EMANE 802.11 CORE Python script example
  * expose transport XML block generation to EmaneModels
  * expose NEM entry to the EmaneModel so it can be overridden by a model
  * add the control interface bridge prior to starting EMANE, as some models may depend on the controlnet functionality
  * added EMANE model to CORE converter
  * parse lat/long/alt from node messages, for moving nodes using command-line
  * fix [bug #196](https://code.google.com/p/coreemu/issues/detail?id=196) incorrect distance when traversing UTM zones

**GUI**
  * added Cut, Copy, and Paste options to the Edit menu; paste will copy selected services and take care of node and interface renumbering
  * implement Edit > Find dialog for searching nodes and links
  * when copying existing file for a service, perform string replacement of: "~", "%SESSION%", "%SESSION\_DIR%", "%SESSION\_USER%", "%NODE%", "%NODENAME%"
  * use CORE\_DATA\_DIR insteadof LIBDIR
  * fix Adjacency Widget to work with OSPFv2 only networks

**BUILD**
  * build/packaging improvements for inclusion on Debian
  * fix error when running scenario with a mobility script in batch mode
  * include Linux kernel patches for 3.8
  * renamed core-cleanup.sh to core-cleanup for Debian conformance
  * don't always generate man pages from Makefile; new manpages for coresendmsg and cored

**BUGFIXES**
  * don't auto-assign IPv4/IPv6 addresses when none received in Link Messages (session reconnect)
  * fixed lock view
  * fix GUI spinbox errors for Tk 8.5.8 (RHEL/CentOS 6.2)
  * fix broker node count for distributed session entering the RUNTIME state when (non-EMANE) WLANs or GreTapBridges are involved
  * fix "file exists" error message when distributed session number is re-used and servers file is written
  * fix [bug #194](https://code.google.com/p/coreemu/issues/detail?id=194) configuration dialog too long, make dialog scrollable/resizable
  * allow float values for loss and duplicates percent
  * fix the following bugs: 166, 172, 177, 178, 192, 194, 196, 201, 202, 205, 206, 210, 212, 213, 214, 221

Known issues with 4.6:
  * [bug #237](https://code.google.com/p/coreemu/issues/detail?id=237) build error "dereferencing type-punned pointer will break strict-aliasing rule" under GCC 4.7.x (Fedora, OpenSUSE, etc.) -- see [bug #238](https://code.google.com/p/coreemu/issues/detail?id=238) for workaround
  * HTTP apache2 service fails to start - fixed in [r452](https://code.google.com/p/coreemu/source/detail?r=452) 2013-10-22


---

# 4.5 #
CORE 4.5 was released on 4/11/13. Packages are available for the following Linux distributions:
  * debs for Ubuntu 12.10 (quantal), 12.04 (precise)
  * RPMs for Fedora 18 (fc18), 17 (fc17)
  * RPMs CentOS/RHEL/Scientific Linux 6.4 (el6)

The high-level changes since version 4.4 are:

**GUI**
  * improved behavior when starting GUI without daemon, or using File New after connection with daemon is lost
  * fix various GUI issues when reconnecting to a session
  * support 3D GUI via output to SDT3D
  * added "Execute Python script..." entry to the File Menu
  * support user-defined terminal program instead of hard-coded xterm
  * added session options for "enable RJ45s", "preserve session dir"
  * added buttons to the IP Addresses dialog for removing all/selected IPv4/IPv6
  * allow sessions with multiple canvases to enter RUNTIME state
  * added "--addons" startup mode to pass control to code included from addons dir
  * added "Locked" entry to View menu to prevent moving items
  * use currently selected node type when invoking a topology generator
  * updated throughput plots with resizing, color picker, plot labels, locked scales, and save/load plot configuration with imn file
  * improved session dialog

**EMANE**
  * EMANE 0.8.1 support with backwards-compatibility for 0.7.4
  * extend CommEffect model to generate CommEffect events upon receipt of Link Messages having link effects

**Services**
  * updated FTP service with root directory for anonymous users
  * added HTTP, PCAP, BIRD, RADVD, and Babel services
  * support copying existing files instead of always generating them
  * added "Services..." entry to node right-click menu
  * added "View" button for side-by-side comparison when copying customized config files
  * updated Quagga daemons to wait for zebra.vty VTY file before starting

**General**
  * XML import and export
  * renamed "cored.py" to "cored", "coresendmsg.py" to "coresendmsg"
  * code reorganization and clean-up
  * updated XML export to write NetworkPlan, MotionPlan, and ServicePlan within a Scenario tag, added new "Save As XML..." File menu entry
  * added script\_start/pause/stop options to Ns2ScriptedMobility
  * "python" source sub-directory renamed to "daemon"
  * added "cored -e" option to execute a Python script, adding its session to the active sessions list, allowing for GUI connection
  * support comma-separated list for custom\_services\_dir in core.conf file
  * updated kernel patches for Linux kernel 3.5
  * support RFC 6164-style IPv6 /127 addressing

**ns-3**
  * integrate ns-3 node location between CORE and ns-3 simulation
  * added ns-3 random walk mobility example
  * updated ns-3 Wifi example to allow GUI connection and moving of nodes

  * fixed the following bugs:<br>54, 103, 111, 136, 145, 153, 157, 160, 161, 162, 164, 165, 168, 170, 171, 173, 174, 176, 184, 190, 193</li></ul>

Known issues with 4.5:<br>
<ul><li>known issues will be listed here</li></ul>

<hr />
<h1>4.4 #
CORE 4.4 was released on 9/25/12. Packages are available for Ubuntu 12.04/11.10 and Fedora 17/16.

The high-level changes since version 4.3 are:

**GUI**
  * real-time bandwidth plotting tool
  * added Wireshark and tshark right-click menu items
  * X,Y coordinates shown in the status bar
  * updated GUI attribute option to link messages for changing color/width/dash
  * added sample IPsec and VPN scenarios, how many nodes script
  * added jitter parameter to WLANs
  * renamed Experiment menu to Session menu, added session options
  * use 'key=value' configuration for services, EMANE models, WLAN models, etc.
  * save only service values that have been customized
  * copy service parameters from one customized service to another
  * right-click menu to start/stop/restart each service

**EMANE**
  * EMANE 0.7.4 support
  * added support for EMANE CommEffect model and Comm Effect controller GUI
  * added support for EMANE Raw Transport when using RJ45 devices

**Services**
  * improved service customization; allow a service to define custom Tcl tab
  * added vtysh.conf for Quagga service to support 'write mem'
  * support scheduled events and services that start N seconds after runtime
  * added UCARP service

**Documentation**
  * converted the CORE manual to reStructuredText using Sphinx; added Python docs

**General**
  * Python code reorganization
  * improved cored.py thread locking
  * merged xen branch into trunk
  * added an event queue to a session with notion of time zero
  * added UDP support to cored.py
  * use UDP by default in coresendmsg.py; added '-H' option to print examples
  * enter a bash shell by default when running vcmd with no arguments
  * fixes to distributed emulation entering runtime state
  * write 'nodes' file upon session startup
  * make session number and other attributes available in environment
  * support /etc/core/environment and ~/.core/environment files
  * added Ns2ScriptedMobility model to Python, removed from the GUI
  * namespace nodes mount a private /sys

  * fixed the following bugs:<br>80, 81, 84, 99, 104, 109, 110, 122, 124, 131, 133, 134, 135, 137, 140, 143, 144, 146, 147, 151, 154, 155</li></ul>

Known issues with 4.4:<br>
<ul><li>GUI segfault under Ubuntu 11.10 with Tcl/Tk 8.5.10 (see 4.3 release notes).<br>
</li><li>Fedora 17 stock kernel 3.3.4-6.fc17 is missing the sch_tbf module, needed to properly set up the tc qdiscs; fix by updating to 3.5.3-1.fc17 or newer</li></ul>

<hr />
<h1>4.3 #
CORE 4.3 was released on 3/7/12. Packages are available for Ubuntu 11.10/11.04/10.10/10.04 and Fedora 14/15/16.

The high-level changes since version 4.2 are:
  * EMANE 0.7.2 and 0.7.3 support
  * hook scripts: customize actions at any of six different session states
  * Check Emulation Light (CEL) exception feedback system
  * added FTP and XORP services, and service validate commands
  * services can flag when customization is required
  * Python classes to support ns-3 simulation experiments
  * write state, node X,Y position, and servers to pycore session dir
  * removed over 9,000 lines of unused GUI code
  * performance monitoring script
  * batch mode improvements and --closebatch option
  * export session to EmulationScript XML files
  * basic range model moved from GUI to Python, supports 3D coordinates
  * improved WLAN dialog with tabs
  * added PhysicalNode class for joining real nodes with emulated networks
  * fixed the following bugs:<br>50, 75, 76, 79, 82, 83, 85, 86, 89, 90, 92, 94, 96, 98,<br>100, 112, 113, 116, 119, 120</li></ul>

Known issues with 4.3
  * CORE will segfault under Ubuntu 11.10 with Tcl/Tk 8.5.10. There is a known bug in the tk-8.5.10 package, the workaround is to uninstall it and install tk-8.5.11 from [here](http://www.ubuntuupdates.org/tk8.5); see [bug #90](http://code.google.com/p/coreemu/issues/detail?id=90)
  * the libimg-1.2.4\_8 port under FreeBSD 9.0 causes icons in the CORE GUI to appear in black; there is an error showing gif files; see [bug #108](http://code.google.com/p/coreemu/issues/detail?id=108)
  * the vcore-4.3.zip (md5sum: b10758ede92db7fa8cf1fa6537d88fbb) had a problem where the UUID listed in the VirtualBox XML did not match the vmdk disk file. This was corrected on 3/26/12 (md5sum: 2282036b2cd6416159c70165e824c36d). An old vmdk disk file was used having CORE 4.2 instead of 4.3; this was corrected on 4/2/12 (md5sum: 5d85dc5da5f12f98206905ff772c48f3)
  * CORE 4.3 fails to build under Ubuntu 12.04 due to changes in the Linux 3.2.x include files; this was fixed in [r429](https://code.google.com/p/coreemu/source/detail?r=429), so you can use the development snapshot when using a newer distro.


---

# 4.2 #
CORE 4.2 was released on 8/19/11.

The high-level changes since version 4.1 are:
  * EMANE 0.7.1 support<br>support for Bypass model, Universal PHY, logging, realtime<br>
<ul><li>configurable MAC addresses<br>
</li><li>control interfaces (backchannel between node and host)<br>
</li><li>service customization dialog improved (tabbed)<br>
</li><li>new testing scripts for MDR and EMANE performance testing<br>
</li><li>improved upgrading of old imn files<br>
</li><li>new coresendmsg.py utility (deprecates libcoreapi and coreapisend)<br>
</li><li>new security services, custom service becomes UserDefined<br>
</li><li>new services and Python scripting chapters in manual<br>
</li><li>fixes to distributed emulation, linking tunnels/RJ45s with WLANs/hubs/switches<br>
</li><li>fixed the following bugs:<br>18, 32, 34, 38, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49,<br>52, 53, 55, 57, 58, 60, 62, 64, 65, 66, 68, 71, 72, 74</li></ul>

Known issues with 4.2:<br>
<ul><li>the CORE 4.2 source tarball fails to build on Fedora. <br>Fix this by changing this line in configure.ac: <code>AC_SUBST(libev_CFLAGS,)</code> to <code>AC_SUBST(libev_CFLAGS, [-I/usr/include/libev])</code><br>This was due to an attempt to remove pkg-config as a dependency, but has been fixed in SVN and newer.<br>
</li><li>Fedora 15 does not add the hostname to the 127.0.0.1 localhost line of the <code>/etc/hosts</code> file, which causes CORE to print an error: "couldn't open socket: can't assign requested address" when the execute button is pressed.<br>
</li><li><a href='http://code.google.com/p/coreemu/issues/detail?id=39'>issue 39</a> OSPF hellos become synchronized after 1 hr of running<br>
</li><li><a href='http://code.google.com/p/coreemu/issues/detail?id=56'>issue 56</a> wait for services shutdown<br>
</li><li><a href='http://code.google.com/p/coreemu/issues/detail?id=63'>issue 63</a> EMANE events not restricted to localhost lo device</li></ul>

<hr />
<h1>4.1</h1>
CORE 4.1 was released on 1/5/11.<br>
<br>
The high-level changes since version 4.0 are:<br>
<ul><li>new icons for toolbars and nodes<br>
</li><li>node services introduced, node models deprecated<br>
</li><li>customizable node types<br>
</li><li>traffic flow editor with MGEN support<br>
</li><li>user configs moved from /etc/core/<code>*</code> to ~/.core/<br>
</li><li>allocate addresses from custom IPv4/IPv6 prefixes<br>
</li><li>distributed emulation using GRE tunnels<br>
</li><li>FreeBSD 8.1 now uses cored.py<br>
</li><li>EMANE 0.6.4 support<br>
</li><li>numerous bugfixes</li></ul>

Known issues with 4.1:<br>
<ul><li>kernel hanging under Ubuntu 10.10 <a href='http://code.google.com/p/coreemu/issues/detail?id=38'>issue 38</a>
</li><li>Tunnel node not implemented under FreeBSD <a href='http://code.google.com/p/coreemu/issues/detail?id=37'>issue 37</a>
</li><li>OSPF hellos become synchronized after running emulation for about one hour <a href='http://code.google.com/p/coreemu/issues/detail?id=39'>issue 39</a>
</li><li>CPU widget has been removed<br>
</li><li>Mobility tool has been removed<br>
</li><li>FreeBSD 8.1-RELEASE kernel warns "freed uma keg was not empty" and eventually crashes <a href='http://code.google.com/p/coreemu/issues/detail?id=40'>issue 40</a>
</li><li>"no common network" error when reconnecting to a session having point-to-point links and trying to modify/delete these links<br>
</li><li>imn files made from older CORE versions not working in 4.1; see the <a href='http://coreemu.blogspot.com/2010/11/node-types-and-services.html'>announcement about new CORE services</a>
</li><li><code>sample1.imn</code> hanging under FreeBSD 8.1 with Quagga MDR 1.0. On node7, the <code>vtysh -b</code> command is blocking waiting for ospf6d, which seems to be waiting for a lock</li></ul>

<hr />
<h1>4.0</h1>
CORE 4.0 was released on 8/17/10.<br>
<br>
Here are some of the high-level changes since version 3.5:<br>
<ul><li>Python framework with Linux network namespace (netns) support (Linux netns is now the primary supported platform)<br>
</li><li>ability to close the GUI and later reconnect to a running session (netns only)<br>
</li><li>EMANE integration (netns only)<br>
</li><li>new topology generators, host file generator<br>
</li><li>user-editable Observer Widgets<br>
</li><li>use of /etc/core instead of /usr/local/etc/core<br>
</li><li>various bugfixes</li></ul>

Link to the <a href='http://pf.itd.nrl.navy.mil/pipermail/core-users/2010-August/000209.html'>4.0 release announcement</a>.<br>
<br>
<hr />
<h1>3.5</h1>
CORE 3.5 was released on 9/15/09.<br>
<br>
Link to the <a href='http://pf.itd.nrl.navy.mil/pipermail/core-users/2009-September/000062.html'>3.5 release announcement</a>.<br>
<br>
<hr />
<h1>3.4</h1>
CORE 3.4 was released on 6/23/09.<br>
<br>
Link to the <a href='http://pf.itd.nrl.navy.mil/pipermail/core-users/2009-June/000019.html'>3.4 release announcement</a>.<br>
<br>
<hr />
<h1>3.3</h1>
CORE 3.3 was released on 3/11/09.<br>
<br>
Link to the <a href='http://pf.itd.nrl.navy.mil/pipermail/core-users/2009-March/000010.html'>3.3 release announcement</a>.