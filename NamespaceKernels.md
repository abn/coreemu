This page lists known good/bad kernels for the [Namespaces](Namespaces.md) version of CORE.

Some part of the kernel not working well within namespaces? See the [CustomKernels](CustomKernels.md) page also.

_**Note: these are incomplete lists!**_

# Known good kernels #

| distro | kernel versions | notes |
|:-------|:----------------|:------|
| Ubuntu 10.04 | 2.6.32-21-generic i686,<br>2.6.32-39-generic i686 <table><thead><th> stock kernel, updated kernel </th></thead><tbody>
<tr><td>  </td><td> 2.6.33 </td><td>  </td></tr>
<tr><td> Arch Linux </td><td> 2.6.35.7 </td><td> certain versions </td></tr>
<tr><td> Fedora 14 </td><td>  </td><td> yum update from stock kernel </td></tr>
<tr><td> Fedora 15 </td><td>  2.6.40.3-0.fc15.x86_64, 2.6.41.10-3.fc15.i686 </td><td> yum update from stock kernel </td></tr>
<tr><td> Ubuntu 11.04 </td><td> 2.6.38-core </td><td> CORE patched kernel enables netfilter queuing, IPsec </td></tr>
<tr><td> Ubuntu 11.10 </td><td> 3.0.0-15-generic </td><td>  </td></tr>
<tr><td> Fedora 16 </td><td> 3.1.0-7.fc16.x86_64 </td><td> stock kernel </td></tr>
<tr><td> Ubuntu 12.04 </td><td> 3.2.0-30-generic, 3.2.0-32-generic </td><td>  </td></tr>
<tr><td> Fedora 17 </td><td> 3.5.3-1.fc17.x86_64 </td><td>  </td></tr>
<tr><td> Fedora 18 </td><td> 3.8.8-202.fc18.i686 </td><td>  </td></tr></tbody></table>

<h1>Known bad kernels</h1>
<table><thead><th> distro </th><th> kernel versions </th><th> problem </th><th> workaround/notes </th></thead><tbody>
<tr><td> Ubuntu 10.04 </td><td> 2.6.32-32 </td><td> namespace support not enabled </td><td>  </td></tr>
<tr><td> Ubuntu 10.10 </td><td> 2.6.35-22 </td><td> crashes </td><td> use rc6 or newer from <a href='http://kernel.ubuntu.com/~kernel-ppa/mainline/'>http://kernel.ubuntu.com/~kernel-ppa/mainline/</a> as workaround </td></tr>
<tr><td> Fedora 14, Ubuntu 10.10  </td><td> 2.6.34, 2.6.35, 2.6.35.6-45.fc14.i686, 2.6.35-22-generic  </td><td> (2) IPv6 in namespaces </td><td> CORE has workaround </td></tr>
<tr><td> Fedora 15 </td><td> 2.6.38.6-26.rc1.fc15.x86_64, 2.6.38.6-26.rc1.fc15.i686 </td><td> (1) IPv6 snooping bug </td><td> yum update to fix </td></tr>
<tr><td> <a href='http://www.4tiitoo.com/'>WeTab Linux</a> </td><td> 2.6.42-66672.1.28-wetab </td><td> namespace support not enabled </td><td>  </td></tr>
<tr><td>  </td><td> 3.3 </td><td> unable to rename interface installed into namespace </td><td> see patch in CORE source </td></tr>
<tr><td> Fedora 17 </td><td> 3.3.4-5.fc17.i686.PAE </td><td> sample1 fails to start </td><td> yum update </td></tr>
<tr><td> Fedora 18 </td><td> 3.6.10-4.fc18.i686.PAE  </td><td> sample1 fails to start </td><td> yum update </td></tr></tbody></table>

<ol><li>IPv6 snooping bug - OSPFv3 hellos jump off the bridge<br>
<ul><li>Look at output of <code>netstat -s | grep invalid</code> to see "invalid headers"<br>
</li></ul></li><li>Problem with IPv6 addressing in namespaces (CORE SVN snapshot has workaround):<br>
<ul><li>2.6.34 and 2.6.35 seem to have problems (<a href='http://www.spinics.net/lists/netdev/msg137122.html'>possibly related problem</a>) as reported by users <a href='http://pf.itd.nrl.navy.mil/pipermail/core-users/2010-September/000269.html'>here</a>.