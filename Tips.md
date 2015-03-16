Here we're collecting tips for CORE, FreeBSD, and OpenVZ. Please add your tips here.

# CORE tips #
tips for CORE that are not OS-specific

  * if you are drawing a wired topology where a router is connected to a hub/switch and PCs/hosts are connected to that, first draw the link from the router to the switch; this way the PCs/hosts will automatically have a default route to the router

  * the periodic Widgets may consume a lot of CPU cycles, pay attention to the CPU usage in the bottom right of the GUI, especially when running on VMware

## Python tips ##

To run CORE from your home directory:
  1. mkdir ~/mycore
  1. when building CORE, use `DESTDIR=~/mycore make install`
  1. run cored.py or your Python script with: `PATH=~/mycore/usr/local/sbin:$PATH PYTHONPATH=~/mycore/usr/local/lib/python2.6/dist-packages/  python ~/mycore/usr/local/sbin/cored.py`
