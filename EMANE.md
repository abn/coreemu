# General Info #

EMANE is a mobile ad-hoc network emulation system that can be used in conjunction with CORE to provide more complex wireless radio models.

EMANE downloads, documentation, and other information:

  * http://www.nrl.navy.mil/itd/ncs/products/emane
  * https://github.com/adjacentlink/emane/wiki

Tutorial:
  * https://github.com/adjacentlink/emane-tutorial/wiki

# Scripting #
See the [EEL](EEL.md) page for a description of the main scripting format used by EMANE for generating mobility and pathloss events.

# Easy EMANE Install #
Refer to the install steps here:

  * https://github.com/adjacentlink/emane/wiki/Install

**NOTE: below are old instructions for EMANE 0.8.1**


## Fedora ##
These dependencies are automatically resolved if you use `yum localinstall`, so you don't need to do this:
```
# optional!
yum -y install openssl-devel perl-XML-Simple perl-XML-LibXML
```

Here are easy EMANE instructions for Fedora 19 x86\_64. Replace with "i386" if needed. Run these as root.

```
wget http://downloads.pf.itd.nrl.navy.mil/emane/0.8.1-r2/emane-0.8.1-release-2.f19.x86_64.tgz
tar xzf emane-0.8.1-release-2.f19.x86_64.tgz
yum localinstall emane-0.8.1-release-2/RPMS/F19/x86_64/*.rpm
```

## Ubuntu ##

Here are instructions for Ubuntu 12.04. Here we will install ACE (6.1.7) included with the EMANE bundle.

```
# install dependencies
sudo apt-get install libssl-dev libxml-libxml-perl libxml-simple-perl
# download and install EMANE 0.8.1
wget http://downloads.pf.itd.nrl.navy.mil/emane/0.8.1-r2/emane-0.8.1-release-2.ubuntu-12_04.amd64.tgz
tar xzf emane-0.8.1-release-2.ubuntu-12_04.amd64.tgz
sudo dpkg -i emane-0.8.1-release-2/deb/ubuntu-12_04/amd64/*.deb

```

# Easy EMANE Uninstall #

## Fedora ##

Remove everything with one command:

```
rpm -e `rpm -qa | grep -i emane | xargs`
```

Don't forget about the ACE libraries.

```
rpm -e ace-devel ace-gperf ace
```

## Ubuntu ##

```
sudo dpkg -r emane\* python-emane\* libemane\* ace\*
# for Ubuntu 14.04+, dpkg doesn't support wildcards, so use this
# this will prompt to confirm
sudo apt-get purge ^emane\*
```


Or for Ubuntu you can open the Synaptic package manager and choose the "Installed (local or obsolete)" section. Select the various EMANE packages using shift+click, right-click and choose remove.

# Building EMANE from Source #

## Ubuntu ##

Here are additional dependencies if you plan on building EMANE from source:

```
sudo apt-get install autoconf automake libtool libxml2-dev
```

When EMANE is installed from source, XML files may still reference DTDs found in `/usr/share/emane/dtds`. An easy fix for this is to create a symlink:
```
sudo ln -s /usr/local/share/emane /usr/share/emane
```

# Misc Info #

Setting the delay in the RF-PIPE MAC adds a transmission delay, not a propagation delay (as one might consider CORE link delays.) The tx delay is affected by the packet size and transmit rate:

  * **delay**: Defines the delay (usec) that is to be included in the transmission delay. The delay is added to the delay introduced by the bitrate defined above. (from http://downloads.pf.itd.nrl.navy.mil/docs/emane/emane.pdf)


**Event generator**: produces events and is launched by the Event Service XML.<br>
<b>Event Daemon</b> / <b>Event Agent</b>: transport event data from emulation to other app domains; e.g. gpsdlocationagent<br>
<br>
<br>
<h1>Old Notes</h1>

Here are Ubuntu 11.10 build notes for EMANE 0.7.2. EMANE 0.7.3 now has 11.10 debs, so these instructions are obsolete.<br>
<br>
Instructions for building a deb from source on Ubuntu 11.10 Oneiric. (As of this writing, pre-built debs were not available.) Note that the deb build dependencies (e.g. dh-make, etc) are slightly different than the run-time dependencies.<br>
<br>
<pre><code>sudo apt-get install libssl-dev libxml-libxml-perl libxml-simple-perl<br>
sudo apt-get install libace libace-dev dh-make doxygen<br>
tar xzf emane-0.7.1.tgz<br>
cd emane-0.7.1<br>
autoconf &amp;&amp; automake<br>
./configure --prefix=/usr<br>
# to use Ubuntu's libace package, edit the debian/control file<br>
# search and replace "ace" with "libace-dev"<br>
make deb<br>
</code></pre>