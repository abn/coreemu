# Files to Update #

Update these for a new release:
  * update ChangeLog file, ReleaseNotes wiki based on major-changes list and SVN log
  * release date in man pages and changelogs below
  * Debian changelog in packaging/deb/changelog (e.g. `debchange -i`)
  * RPM file list, changelog in packaging/rpm/core.spec.in
  * CORE API version number in gui/api.tcl and daemon/core/api/`*`.py
  * tag release 4.7rc1


# Release Steps #

  1. sync all SVN repos
  1. run source code sanity checks
  1. build distribution tarball
  1. generate PDF and HTML documentation, tgzs
  1. build Linux debs and test
  1. build Linux RPMs and test
  1. make CORE API PDF
  1. update VCORE virtual machine, zip
  1. announcement email
  1. update Quickstart wiki instructions
  1. tag all SVN repos

# Source Code Sanity Checks #

```
svn export . ../core-4.7.export
./bootstrap.sh && ./configure && make -j8 && make dist
make distclean
./bootstrap.sh clean
cd ..
tar xzf core/core-4.7.tar.gz
# does this show any missing files?
# generated files (such as ./configure) exist in the tarball
diff -qr core-4.7.export core-4.7

# check for files not checked in to a subversion tree
cd core
make
make distclean
./bootstrap.sh clean
svn status

# on a fresh system, check for changed files:
make install
make uninstall
```

# Build Documentation #

This results in core-html-4.7.tgz, core-python-html-4.7.tgz, and core\_manual\_4\_7.pdf files.

```
cd core-4.7/doc
make html
mv _build/html core-html-4.7
tar czf ../../core-html-4.7.tgz core-html-4.7
make latexpdf
mv _build/latex/CORE.pdf ../../core_manual_4_7.pdf

cd core-4.7/daemon/doc
make
make html
mv _build/html core-python-html-4.7
tar czf ../../../core-python-html-4.7.tgz core-python-html-4.7
```

# Build Ubuntu/Debian Packages #

0. One time machine prep

```
sudo apt-get install ubuntu-dev-tools devscripts debhelper pbuilder
# add "export http_proxy=http://proxy.server.com:8080" to /etc/pbuilderrc
# add export PATH="/usr/sbin:/sbin/:$PATH"
# add COMPONENTS="main restricted universe multiverse" to pbuilderrc
sudo pbuilder update --override-config
pbuilder-dist precise amd64 create
```

1. Build a debian source package.

```
tar xzf core-4.7.tar.gz
cd core-4.7
./bootstrap.sh
./configure --prefix=/usr
# this puts Debian control files in a debian subdir
make deb
cd ..
# NOTE: deb file (+ other files) will appear in .
dpkg-source -b core-4.7
debsign core_4.7-0ubuntu1.dsc
```

2. Use pbuilder-dist for cross-compiling various distributions/architectures.

Resulting dsc, changes, and tar.gz files can be copied to pbuilder machine(s). Distro names include: precise quantal raring saucy trusty

```
pbuilder-dist precise amd64 build core_4.7-0ubuntu1.dsc
pbuilder-dist precise i386 build core_4.7-0ubuntu1.dsc
```

rename result from ~/pbuilder/precise-amd64\_result/core-daemon\_4.7-0ubuntu1\_amd64.deb core-daemon\_4.7-0ubuntu1\_precise\_amd64.deb.

# Build RPM Packages #

0. One time machine prep.

```
yum install rpmdevtools mock
```

Add `proxy=http://proxy.company.com:1080/` and `timeout=300` lines as necessary to `/etc/mock/*.cfg` files.

1. Build a source RPM.

```
tar xzf core-4.7.tar.gz
cd core-4.7
./bootstrap.sh
./configure --prefix=/usr --with-startup=systemd
#make dist  # re-use original dist tarball here
cp ../core-4.7.tar.gz .
make rpm
# source RPM is in ~/rpmbuild/SRPMS/core-4.7-1.src.rpm
```

2. Use mock for cross-compiling various distributions/architectures.

```
mock -r fedora-20-x86_64 rebuild core-4.7-1.src.rpm
cp /var/lib/mock/fedora-20-x86_64/result/core-daemon-4.7-1.x86_64.rpm   ~/core-daemon-4.7-1.fc20.x86_64.rpm

mock -r fedora-20-i386 rebuild core-4.7-1.src.rpm
cp /var/lib/mock/fedora-20-i386/result/core-daemon-4.7-1.i686.rpm
  ~/core-daemon-4.7-1.fc20.i686.rpm

mock -r epel-7-x86_64 rebuild core-4.7-1.src.rpm
cp /var/lib/mock/epel-7-x86_64/result/core-daemon-4.7-1.x86_64.rpm   ~/core-daemon-4.7-1.el7.x86_64.rpm

```

3. Use mock to build el6 RPMs.

  * edit spec file, remove './configure --with-startup=systemd' option
  * replace '/etc/systemd/system/core-daemon.service' with 'etc/init.d/core-daemon'
  * replace 2.7 with 2.6 as Python version
  * rebuild source RPM using 'make rpm'

```
make rpm
cp ~/rpmbuild/SRPMS/core-4.7-1.src.rpm ./core-4.7-1.rhel.src.rpm
mock -r epel-6-x86_64 core-4.7-1.rhel.src.rpm
cp /var/lib/mock/epel-6-x86_64/result/core-daemon-4.7-1.x86_64.rpm core-daemon-4.7-1.el6.x86_64.rpm
```

Distros are epel-7-x86\_64 epel-7-i386 epel-6-x86\_64 epel-6-i386.


4. Sign the RPMs.

```
rpm --addsign *.rpm
```