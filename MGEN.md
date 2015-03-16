MGEN is an open source traffic generator developed by NRL, available here: [MGEN page at NRL](http://www.nrl.navy.mil/itd/ncs/products/mgen). You can use MGEN to easily create many different types of unicast or multicast IPv4/IPv6 traffic flows. Here is the [MGEN Manual](http://downloads.pf.itd.nrl.navy.mil/docs/mgen/mgen.html) for usage details.

# Installation #

  1. Download MGEN source from here:
> > http://downloads.pf.itd.nrl.navy.mil/mgen/nightly_snapshots/mgen-svnsnap.tgz
  1. Download Protolib source from here:
> > http://downloads.pf.itd.nrl.navy.mil/protolib/nightly_snapshots/protolib-svnsnap.tgz
  1. Unpack the tarballs; this will create `mgen` and `protolib` directories:
```
> cd ~
> tar xzf mgen-svnsnap.tgz
> tar xzf protolib-svnsnap.tgz
```
  1. Create a symbolic link to protolib from mgen for building properly (see README.TXT for more information on why you need to do this)
```
> cd mgen
> ln -s ../protolib
```
  1. if using FreeBSD, fix the MGEN makefile; edit the file `~/mgen/makefiles/Makefile.freebsd` and change the line `SYSTEM_LIBS = -lc_r` to `SYSTEM_LIBS = -lpthread`
  1. Build MGEN; use the Makefile for your OS
```
> cd ~/mgen/makefiles
> make -f Makefile.linux
> # or, if FreeBSD use this:  make -f Makefile.freebsd
```
  1. Install it
```
> sudo install -g root -o root -m 555 mgen /usr/local/bin
> sudo install -g root -o root -m 555 mpmgr /usr/local/bin
```


# Sample Scenario #

There is a `sample5-mgen.imn` scenario file included with CORE that demonstrates how to start a traffic flow from one node to another upon startup. You can find this file in the directory `core/gui/configs/sample5-mgen.imn` from the latest SVN snapshot tarball [here](http://downloads.pf.itd.nrl.navy.mil/core/source/nightly_snapshots/core-svnsnap.tgz).