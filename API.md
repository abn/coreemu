# Introduction #

The CORE API is a sockets-based messaging system used by different CORE components to communicate. Other external programs can use the CORE API to interact with the emulation.

Messaging is asynchronous. Responses generally may be requested through message flags. Each message consists of a header and one or more TLV (Type, Length, Value) tuples. Currently each message defines its own TLV types. Messages are sent over a connected TCP socket. Sockets are used for their portability across platforms, and so individual  components can communicate across IP networks.


# Quick Reference #

**Note: this table has not been updated recently for API ver 1.16/1.17**

| **Message Type** | **Uses** |
|:-----------------|:---------|
| Node | Create, modify, and delete emulated nodes. Can move nodes around by including coordinates. |
| Link | Link nodes together, unlink them, or modify link parameters. |
| Execute | Run a command on a node, may request the response from the command output. |
| Register | Register a plugin with another plugin, and communicate capabilities (or models) between plugins. |
| Config | Exchange configuration information for a capability of a plugin. |
| File | Transfer a file or file location. |

<br>

<h1>Details</h1>

<ul><li>the current CORE API spec can be found in PDF format here: <a href='http://pf.itd.nrl.navy.mil/core/core_api.pdf'>http://pf.itd.nrl.navy.mil/core/core_api.pdf</a>
</li><li><a href='APIExamples.md'>API Examples</a> shows samples of how the API is used</li></ul>

<h1>Archived API Specs</h1>

<ul><li><a href='http://hipserver.mct.phantomworks.org/core/files/CORE_API_1_14.pdf'>CORE_API_1_14.pdf</a>
</li><li><a href='http://hipserver.mct.phantomworks.org/core/files/CORE_API_1_15.pdf'>CORE_API_1_15.pdf</a>
</li><li><a href='http://hipserver.mct.phantomworks.org/core/files/CORE_API_1_16.pdf'>CORE_API_1_16.pdf</a>