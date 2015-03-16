# Introduction #

NRL SMF provides Simplified Multicast Forwarding (RFC 6621).


# Installation #
You need to compile NRLSMF from source in order to install it.

  1. Download the NRLSMF nightly snapshot from: http://downloads.pf.itd.nrl.navy.mil/smf/nightly_snapshots/
  1. Download the protolib nightly snapshot to go with it: http://downloads.pf.itd.nrl.navy.mil/protolib/nightly_snapshots/
  1. Unpack NRLSMF: `tar xvf ~/Downloads/nrlsmf-svnsnap.tgz`
  1. Unpack protolib: `tar xvf ~/Downloads/protolib-svnsnap.tgz`
  1. Create symlink: `cd nrlsmf; ln -s ../protolib`
  1. Build it: `cd unix; make -f Makefile.linux`
  1. Install it: `sudo cp nrlsmf /usr/local/sbin`