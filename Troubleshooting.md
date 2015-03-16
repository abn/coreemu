## General ##
  * first check the known issues under [ReleaseNotes](ReleaseNotes.md) for the version of CORE you are using!
  * [search the mailing list](http://www.google.com/search?q=site%3Ahttp%3A%2F%2Fpf.itd.nrl.navy.mil%2Fpipermail%2Fcore-users%2F&oq=site%3Ahttp%3A%2F%2Fpf.itd.nrl.navy.mil%2Fpipermail%2Fcore-users%2F)
  * also be sure to check out the [bug tracker](http://code.google.com/p/coreemu/issues/list)

## Linux version ##
  * **log files:**
    * `/var/log/core-daemon.log` - CORE Python daemon log file
    * `/tmp/pycore.nnnnn/nX.log` - each node has a log file (where `nnnnn` is session number and `X` is the node number)

  * **Issue:** quagga does not start successfully.  When I enter into a namespace context and type "zebra -d", the following error occurs:
```
zebra: error while loading shared libraries: libzebra.so.0: cannot open shared object file: No such file or directory
```
  * **Resolution:**  If you have installed quagga without running ldconfig afterwards, the path may have not been picked up.  Try running "ldconfig".  Then, run "ldconfig -p | grep libzebra" to make sure it took effect, and restart core.

## FreeBSD version ##
  * **debugging commands:**
    * `vimage -l` - lists all running vimages
    * `ngctl list` - list all Netgraph nodes
    * `ngctl show ...` - detailed information about a Netgraph node