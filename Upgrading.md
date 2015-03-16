# Release or Development Version? #

CORE releases are numbered such as CORE 4.5, CORE 4.6, etc. They can be found as [packages](http://downloads.pf.itd.nrl.navy.mil/core/packages/) or [source](http://downloads.pf.itd.nrl.navy.mil/core/source/). [Development snapshots](http://downloads.pf.itd.nrl.navy.mil/core/source/nightly_snapshots/) are created nightly as source tarballs. In addition to this development snapshot, the latest code (or specific SVN revision numbers) can be obtained by clicking the [source](https://code.google.com/p/coreemu/source/checkout) tab above.

Choose the latest CORE release if:
  * you want a more stable, tested CORE
  * you would like to use pre-built binary packages
  * you want documentation

Choose the development snapshot if:
  * you want bleeding edge features
  * you desire some functionality or bugfix described on the CORE mailing list, blog, or wiki
  * you're running with the latest Linux distro that is not supported by the latest release
  * you're comfortable compiling and installing the CORE source

# Switching Versions #

First, uninstall the previously installed version.

To uninstall the CORE package, use a command such as:
```
  # Ubuntu package uninstall
  dpkg -r core-daemon-4.6 core-gui-4.6
```
```
  # Fedora package uninstall
  rpm -e core-daemon-4.6 core-gui-4.6
```

To uninstall from the CORE source, change to your CORE source build directory and use `sudo make uninstall`:
```
  # source uninstall
  cd core-4.6
  sudo make uninstall
```

Uninstalling should not remove your user settings or `imn` scenario files. See the notes in the section below.

Next, follow the installation instructions in the [Quickstart](Quickstart.md) or CORE manual to install from packages or source.

# Compatibility Notes #

Notes on `imn` file compatibility will be listed here.

When upgrading, you may want to back up your `.core` user directory or temporarily move it out of the way. This allows the GUI to re-create default settings