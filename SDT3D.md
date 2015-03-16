# CORE and SDT3D #

SDT3D is a three-dimensional virtual globe GUI developed by NRL, based on NASA's [WorldWind Java SDK](http://worldwind.arc.nasa.gov/java/). The primary SDT3D website is:
  * http://www.nrl.navy.mil/itd/ncs/products/sdt

CORE can export its nodes and links so they appear in the 3D space. The Canvas Size and Scale dialog governs the origin latitude/longitude/altitude for the 2D canvas. Although the 2D canvas has a fixed altitude, EMANE-based mobility script could be used to move nodes around in three dimensions.

# SDT3D Ubuntu 12.04 install #

Notes on installing SDT3D under Ubuntu 12.04 amd64.

Successful build/run environment:
  1. Ubuntu 12.04 x86\_64 updated 2012-10-12
  1. OpenJDK 1.7 (tested 7u7-2.3.2a) **Note:** Oracle Java 1.6/1.7 and OpenJDK 1.6 will not work when behind a proxy, but segfault
  1. sdt-svnsnap.tgz (dated after 2014-09-24) from http://downloads.pf.itd.nrl.navy.mil/sdt/nightly_snapshots/sdt-svnsnap.tgz

Steps for build and run:

  * Install some pre-requisites.  (A helper script prep-dependencies-linux is also available):
```
sudo apt-get install openjdk-7-jdk g++
# The following packages are required unless you are using WorldWind and joglutils dependencies provided by NRL.
sudo apt-get install git
sudo apt-get install ant
sudo apt-get install g++
sudo apt-get install wget
sudo apt-get install libvecmath-java
sudo apt-get install libpcap-dev
sudo apt-get install libnetfilter-queue-dev
```

  * set JAVA\_HOME environment variable; also ensure that javac, java, and jar point to the OpenJDK 1.7 install
```
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/
# this should point to the above JAVA_HOME version
update-alternatives --display java
update-alternatives --display javac
update-alternatives --display jar
# if you need to change the above, use the following for each:
# sudo update-alternatives --config java
```

  * unpack SDT snapshot tarball
```
tar xzf sdt-svnsnap.tgz
```

  * build SDT3D
```
# By default sdt3d will download (and patch where applicable) joglutils, WorldWind, and 
# protolib from publicly available sites. If you prefer to use dependencies provided by NRL 
# get protolib.tgz, worldwind.zip, and joglutils.zip from the sdt file release section (login 
# required) and put them in the sdt3d/makefiles directory.
cd sdt/makefiles
make -f Makefile.linux_<amd64|i586> sdt3d
```
  * if something goes wrong with the above build step, you may need to recover like this:
```
# Assume we are in sdt/makefiles
make -f Makefile.linux distclean
```

  * a successful build results in a file **sdt3d.zip**; unzip this somewhere to continue
```
cd ~
unzip sdt/makefiles/sdt3d.zip
```

  * now you should be ready to run SDT3D. It helps to first have an Internet connection (no proxy) to populate the map cache. Also helpful if you use the proprietary NVIDIA drivers (`sudo apt-get install nvidia-current`) or ATI drivers.
```
cd ~/sdt3d
./sdt3d.sh
```

  * you can launch SDT3D from within CORE by choosing _View_ > _3D GUI..._. By default, CORE looks for a SDT3D launch script in `/usr/local/bin/sdt3d.sh`.  Use the sdt3d makefile to install the script:
```
# Install sdt3d.sh in /usr/local/bin/sdt3d.sh
# The sdtcmd utility will also be installed if it has been built
sudo make -f Makefile.linux_<amd64|i586> install
```

  * By default the sdt3d.sh script looks for the sdt3d directory in /home/$USER and listens to tcp port 50000.  Modify as appropriate for your system configuration.  The default core port may need to be changed to 5000 in earlier core releases, e.g.
```
#!/bin/sh
THISDIR=/home/$USER
java -jar -Dsun.java2d.noddraw=true -Djava.library.path=${THISDIR}/lib 
-Xmx512m ${THISDIR}/jars/sdt3d.jar LISTEN TCP,5000 $@

```

  * Alternatively, rather than passing the listen command to sdt3d.sh as a command line argument in the script, you may create a ~/.config/sdt3d/sdt.settings file to listen to tcp and set up any other default sdt configuration commands.  (The .config directory is created automatically when sdt3d is first launched if it does not already exist).
```
# Any valid sdt commands may be placed in the sdt.settings file
echo listen tcp,50000 > ~/.config/sdt3d/sdt.settings
```

  * you can copy the WorldWind map cache between systems (helpful if you do not always have an Internet connection), look in `~/var/cache/WorldWindData`.  sdt3d provides an "off line mode" to disable queries to the world wind servers.  There is a tile bulk download utility available in the sdt3d makefiles directory to build up an offline cache.