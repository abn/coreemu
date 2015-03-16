# How to get and read API traces #
This page is provided to help you understand how the CORE API is used among the various CORE components called plugins. To start with, the interaction between the GUI and CORE daemon (cored) is explored.

From the CORE GUI, choose _View_ -> _Show_ -> _API Messages_. Once this option is turned on, you will see trace output such as the lines listed below. This option will be saved to the .imn configuration file.

## Output ##
Each line of console output starting with a **>** character indicates an outgoing CORE API message that has been sent by the GUI:
```
>NODE(n0,type=0x0,n0,m=1,x=115,y=169)
```

## Input ##
When an API message has been received, the GUI prints the following lines to the console, indicating the number of bytes read, and the type/flags/length parsed from the API message header.
```
read 16 bytes (type=1, flags=9, len=16)...
NODE(num=0,emuid=1000,) 
```


# Two-node example #

The API is used here to create two router nodes that are linked together with a wired Ethernet link.

![http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_two_node.png](http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_two_node.png)

Here is the API trace output from the CORE GUI for this two-node wired scenario, when the user presses the **Start** button to instantiate the emulation.

```
>NODE(n0,type=0x0,n0,m=1,x=115,y=169)
>NODE(n1,type=0x0,n1,m=1,x=374,y=162)
>LINK(flags=1,0-1,type=1,if1n=0,10.0.0.1/24,a:0::1/64,if2n=0,10.0.0.2/24,a:0::2/64,)
>FILE(flag=1,n0,f=/etc/quagga/Quagga.conf,src=/tmp/core_n0.conf)
>FILE(flag=1,n1,f=/etc/quagga/Quagga.conf,src=/tmp/core_n1.conf)
read 16 bytes (type=1, flags=9, len=16)...
NODE(num=1,emuid=1001,) 
read 16 bytes (type=1, flags=9, len=16)...
NODE(num=0,emuid=1000,) 
```

  1. the first two NODE messages have the ADD flag set, instructing cored to create nodes n0 and n1
  1. the LINK message has the ADD flag set, and instructs cored to link together nodes n0 and n1; it creates interfaces eth0 on both nodes and assigns IPv4 and IPv6 addresses
  1. the two FILE messages tell cored to copy the _/tmp/core\_n0.conf_ file to _/etc/quagga/Quagga.conf_ for node n0 and then for n1
  1. when the CORE GUI sends the first two NODE messages, it draws a red square around nodes n0 and n1 indicating to the user that the node startup is pending; when cored decides that the nodes have started, it replies back to the CORE GUI with a NODE message containing the emulation ID of the running node; the red square then turns green and disappears. These last two NODE messages perform that function.

Some time later, the user presses the **Stop** button on the CORE GUI. Here is the output from that event:
```
>EXEC(flag=0,n0,n=101,cmd='killall -q ospf6d ospfd bgdpd ripd ripngd zebra vtysh xorp_rtrmgr racoon tcpdump ping traceroute')
>EXEC(flag=0,n1,n=102,cmd='killall -q ospf6d ospfd bgdpd ripd ripngd zebra vtysh xorp_rtrmgr racoon tcpdump ping traceroute')
>LINK(flags=2,0-1,type=1,if1n=0,10.0.0.1/24,a:0::1/64,if2n=0,10.0.0.2/24,a:0::2/64,)
>NODE(flags=del/str,0)
>NODE(flags=del/str,1)
read 8 bytes (type=1, flags=10, len=8)...
NODE(num=0,) 
read 8 bytes (type=1, flags=10, len=8)...
NODE(num=1,) 
```

  1. as part of the shutdown sequence, the CORE GUI kills running processes on each node; this is performed by sending cored the first two EXEC messages containing the _killall_ command. Each EXEC message is given a sequence number (101, 102, etc...) so the response output can be matches.
  1. the LINK message with flags=2 instructs cored to remove the link between n0 and n1
  1. the two NODE messages that follow with flags=del instruct cored to shutdown nodes n0 and n1; the str "status request" flag tells cored that the status is requested once the node shutdown completes
  1. similar to startup, the GUI draws a red square around each node indicating the request to shutdown each node; the last two NODE messages have the str flag set and are the response to the node shutdown request; the red squares are then removed in the GUI

# Three wireless nodes example #

Here the API is used to create three wireless router nodes joined to the same wireless network. One of the nodes is then moved out of range of another.

![http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_three_wireless.png](http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_three_wireless.png)

Three nodes connected to a wireless LAN (WLAN), and the built-in on/off model is being used. (This is the simplest wireless model where the GUI calculates the range between nodes and tells cored to link or unlink nodes as needed.) This is the API trace once the user presses the **Start** button.

```
>NODE(n0,type=0x0,n0,m=0,x=181,y=82)
>NODE(n1,type=0x0,n1,m=0,x=107,y=172)
>NODE(n2,type=0x0,n2,m=0,x=318,y=54)
>NODE(n3,type=0x6,wlan3,x=34,y=39)
>LINK(flags=1,3-0,25000,54000000,type=0,if2n=0,10.0.0.1/32,a:0::1/128,)
>LINK(flags=1,3-1,25000,54000000,type=0,if2n=0,10.0.0.2/32,a:0::2/128,)
>LINK(flags=1,3-2,25000,54000000,type=0,if2n=0,10.0.0.3/32,a:0::3/128,)
>FILE(flag=1,n0,f=/etc/quagga/Quagga.conf,src=/tmp/core_n0.conf)
>FILE(flag=1,n1,f=/etc/quagga/Quagga.conf,src=/tmp/core_n1.conf)
>FILE(flag=1,n2,f=/etc/quagga/Quagga.conf,src=/tmp/core_n2.conf)
>LINK(flags=1,0-1,type=0,)
>LINK(flags=1,0-2,type=0,)
>LINK(flags=1,1-2,type=0,)
read 16 bytes (type=1, flags=9, len=16)...
NODE(num=0,emuid=1000,) 
read 16 bytes (type=1, flags=9, len=16)...
NODE(num=1,emuid=1001,) 
read 16 bytes (type=1, flags=9, len=16)...
NODE(num=2,emuid=1002,) 
```

  1. the WLAN cloud is treated as node n3, and created along with the other nodes using the NODE messages
  1. the first three LINK messages join each router node to the WLAN node; they assign an interface eth0 with IPv4 and IPv6 addresses
  1. the FILE messages are used to configure each router node as before
  1. the last three LINK messages are the result of the GUI's range calculation; they tell cored how the nodes are wirelessly linked together; because the GUI calculated the links, it knows where to draw the green lines without receiving any message about the links

Some time later, the user clicks and drags node n2 to the right. This breaks the link between node n1 and n2.

![http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_three_wireless2.png](http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_three_wireless2.png)

```
>LINK(flags=2,1-2,type=0,)
```

The LINK message with the delete flag "unlinks" nodes 1 and 2.

# Model configuration example #

The API is used here to configure the "simple" wireless model from the GUI. The GUI has no knowledge of available capabilities from the cored plugin, and queries cored for the capability list. The GUI does not know what configuration parameters are available for each capabilitiy, these are communicated from cored to the GUI using the CONFIG message.

![http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_config.png](http://hipserver.mct.phantomworks.org/core/images/wiki/api_example_config.png)

The screenshot above shows the configuration dialog that the GUI dynamically builds based on the CONFIG message it receives from cored. This trace comes from the GUI in edit mode, when clicking on a WLAN node wlan0, and configuring the "simple" wireless model.

```
>CONF(simple,0)
read 244 bytes (type=5, flags=0, len=244)...
CONF(node=0/simple,types=3/3/3/4/4/2/2/2/2/3/2/2/,vals=200|350|5|50000|500000|0|15|0|0|0|0|0,capt=min range|max range|motion threshhold|min delay|max delay|min loss|max loss|burst|duplicates|jitter|multicast loss|multicast burst,bitmap,) 
>CONF(simple,0, <config data>)
```

  1. the first CONFIG message is sent from the GUI to cored requesting the configuration parameters for node n0 for the capability named "simple". It may be that n0 has never been configured, then cored responds with the default values.
  1. each CONFIG message has a "types=" section listing the data types to be configured; these are numbers that determine the type, see the API document for details; for example, the type number 3 indicates a 32-bit unsigned value
  1. the CONFIG message has a "vals=" section listing the value displayed for that type, and a "capt=" section listing the captions for each value
  1. when the user presses OK, the values entered by the user are sent to cored in another CONFIG message; these values are represented with the <config data> block

Once the user presses the **Start** button, the following trace is printed. Here wlan0 is linked with three router nodes.

```
>CONF(simple,0, <config data>)
>CONF(simple,0)
>NODE(1,x=82,y=219,emuid=12853,netid=12837)
>NODE(2,x=224,y=136,emuid=12866,netid=12837)
>NODE(3,x=411,y=89,emuid=12879,netid=12837)
read 64 bytes (type=2, flags=1, len=64)...
LINK(node1num=2,node2num=1,delay=50000,bw=0,per=0,dup=0,jitter=0,netid=12837,) 
read 64 bytes (type=2, flags=1, len=64)...
LINK(node1num=3,node2num=2,delay=50000,bw=0,per=0,dup=0,jitter=0,netid=12837,) 
```

  1. the first CONFIG message sends the configuration for wlan0 to cored. This is required because the user may have just opened a scenario file and clicked Start, and cored would have no knowledge of wlan0 or its configuration
  1. the second CONFIG message is sent from the GUI on FreeBSD. The GUI has instantiated the wlan0 node and sends its emulation ID to cored. (In FreeBSD the emuid corresponds to the Netgraph ID.) cored will copy the config from wlan0 to an instance of n0.
  1. the other NODE and LINK messages are similar to the wireless scenario described previously. Note that now the simple model is calculating the range between nodes and the GUI only draws green link lines as it receives LINK messages from cored