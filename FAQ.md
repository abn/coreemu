## 1. What is CORE? ##

The Common Open Research Emulator (CORE) is a tool for emulating networks. You can turn one or more physical machines into networks of virtual nodes and connect these emulated networks to live networks. CORE consists of a GUI for drawing topologies of lightweight virtual machines, and Python modules for scripting network emulation. CORE uses Linux LXC virtualization (Linux containers, aka network namespaces) together with Linux bridging, EMANE, or ns-3 for networking.

## 2. Who produced CORE? ##

CORE is derived from the [Integrated Multiprotocol Network Emulator/Simulator](http://www.tel.fer.hr/imunes/) (IMUNES) emulator. CORE was developed primarily by [Jeff Ahrenholz](mailto:jeffrey.m.ahrenholz@boeing.com) from Boeing Research & Technology. Boeing Research & Technology has contributed CORE as an open source project, and the [Naval Research Laboratory](http://www.nrl.navy.mil/itd/ncs/products/) (NRL) has graciously agreed to host CORE source code and mailing lists as part of a broader Mobile Network Modeling effort.

See the [Prior Work](http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/intro.html#prior-work) and [Acknowledgements](http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/credits.html) section in the CORE manual for more details.

## 3. Who maintains CORE? ##

CORE is an open source project led by [Jeff Ahrenholz](mailto:jeffrey.m.ahrenholz@boeing.com).  Boeing Research & Technology is supporting CORE in 2012 through a project led by Jeff.

## 4. How is CORE licensed? ##

CORE is open source software, licensed under the [3-clause BSD license](http://en.wikipedia.org/wiki/BSD_license).

## 5. Are there any technical publications about CORE? ##

  * J. Ahrenholz, T. Goff, and B. Adamson, Integration of the CORE and EMANE Network Emulators, Proceedings of IEEE Military Communications Conference 2011 (MILCOM), pp. 1870-1875, November 2011.
  * J. Ahrenholz, Comparison of CORE Network Emulation Platforms, Proceedings of IEEE Military Communications Conference 2010 (MILCOM), pp. 864-869, November 2010.
  * J. Ahrenholz, C. Danilov, T. Henderson, and J.H. Kim, CORE: A real-time network emulator, Proceedings of IEEE Military Communications Conference 2008 (MILCOM), pp. 221-227, November 2008.

## 6. How many nodes can CORE emulate? ##

This is a popular question for which there is no easy answer. It depends on what the nodes are doing, what the host system resources are, etc. CORE is primarily focused on efficient, scalable network emulation for tens of nodes on standard laptops to hundreds of nodes on larger servers. It is a research question as to how to support larger topologies, portions of which are distributed on different hosts. CORE already allows these large topologies to be constructed and centrally managed; see the [Distributed Emulation](http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/usage.html#distributed-emulation) section of the CORE manual.

## 7. How can I contribute? ##

See the [Contributing](Contributing.md) page.

CORE is open source; if you find CORE useful for your work, please contribute back to the project. Contributions can be as simple as reporting a bug, dropping a line of encouragement or technical suggestions to the [mailing](http://pf.itd.nrl.navy.mil/mailman/listinfo/core-users) [lists](http://pf.itd.nrl.navy.mil/mailman/listinfo/core-dev), or can also include submitting patches or maintaining aspects of the tool. Patches must conform to the [code standards guide](Hacking.md). Please contact [Jeff Ahrenholz](mailto:jeffrey.m.ahrenholz@boeing.com) if you would like to get involved in some way.

## 8. Why doesn't ping work? ##

There are a variety of reasons why one node in CORE cannot ping another. First start with the sample imn configs if you have a new CORE installation. Make sure that you have installed Quagga for routing as mentioned in the [Quickstart](Quickstart.md). For new installations, check that iptables is not blocking traffic on the host bridges (`iptables -L`). For MDR wireless routers, you'll need to install OSPF MDR as described in the manual. It may be helpful to `tcpdump` along the expected path to determine connectivity and routing information.