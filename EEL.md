# EEL #

EEL is the Emulation Event Log, a text file format for scripting emulation events or logging them. A more verbose description can be found on the [mnmtools](http://pf.itd.nrl.navy.mil/mnmtools/) (Mobile Network Modeling Tools) page.


# EEL location events #
A location event determines the absolute position of a node at the given time. This is in contrast to a waypoint destination and velocity vector. When used with EMANE's Universal PHY, you would set the _pathloss mode_ to _**freespace**_ or _**2ray**_.

## GPS coordinates ##

The fields for GPS location events are: _time, NEM ID, "location", "gps", latitude, longitude, altitude_

```
0.0 nem:1 location gps 40.031075,-74.523518,3.000000
0.0 nem:2 location gps 40.031165,-74.523412,3.000000
0.0 nem:3 location gps 40.031227,-74.523247,3.000000
0.0 nem:4 location gps 40.031290,-74.523095,3.000000
```

## Cartesian coordinates ##

The fields for Cartesian coordinate location events are: _time, NEM ID, "location", "cart", x, y, z_

```
0.0 nem:1 location cart 100.00,250.00,50.00
0.0 nem:2 location cart 230.00,261.00,51.00
0.0 nem:3 location cart 497.00,101.00,50.00
0.0 nem:4 location cart 230.00,101.00,45.00
```



# EEL pathLoss events #
A pathloss event specifies the pathloss in decibels (dB).
When used with EMANE's Universal PHY, you would set the _pathloss mode_ to _**pathloss**_. When pathloss mode is used, the location of a node no longer determines connectivity.

A packet received in EMANE has the following received power calculation, that determines whether or not the packet will be dropped:
_Rx Power (dBm) = Tx Power (dBm) + Tx Antenna Gain (dBi) + Rx Antenna Gain (dBi) - **Pathloss (dB)**_

The fields for pathloss events are: _time, source NEM ID, "pathLoss", destination NEM ID,pathloss-value_

Multiple destination NEM IDs may be specified. The example below shows three NEMs, with each line specifying two destination NEMs. Note that loss can be asymmetrical.

```
0.0 nem:1 pathLoss nem:2,96.3 nem:3,95.0
0.0 nem:2 pathLoss nem:1,94.2 nem:3,96.1
0.0 nem:3 pathLoss nem:2,94.9 nem:3,96.3
```

# EEL Comm Effect Events #

# EEL Antenna Profile Events #

The fields for the antenna profile events are: _time, NEM ID, "antennaprofile", profile ID, elevation (degrees), azimuth (degrees)._

```
30.0 nem:1 antennaprofile 1,60.0,0.0
60.0 nem:1 antennaprofile 1,105.0,0.0
60.0 nem:4 antennaprofile 2,45.0,0.0
```

# Example XML configs #
You can start playback of an EEL file using the following:
```
emaneeventservice eventservice.xml
```

Here is a sample `eventservice.xml` file:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE eventservice SYSTEM "file:///usr/share/emane/dtd/eventservice.dtd">
<eventservice name="eeltest" deployment="deployment.xml">
  <param name="eventservicegroup" value="224.1.2.8:45703"/>
  <param name="eventservicedevice" value="lo"/>
  <generator name="Emulation Event Log Generator" definition="eelgenerator.xml"/>
</eventservice>
```

And the sample `eelgenerator.xml` file:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE eventgenerator SYSTEM "file:///usr/share/emane/dtd/eventgenerator.dtd">
<eventgenerator name="emanegeneel" library="eelgenerator">
  <param name="inputfile" value="myscript.eel"/>
  <param name="loader" value="location:eelloaderlocation:full"/>
  <param name="loader" value="pathLoss:eelloaderpathloss:full"/>
</eventgenerator>
```

And a sample `deployment.xml` file:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE deployment SYSTEM "file:///usr/share/emane/dtd/deployment.dtd">
<deployment>
  <platform id="1">
    <nem id="1"/>
    <nem id="2"/>
    <nem id="3"/>
  </platform>
</deployment>
```

Finally, for the above config files you would write a `myscript.eel` file that contains the line-by-line events; you can use the above examples for this.

For more information about what's going on, try log level 4:
```
emaneeventservice -l4 eventservice.xml
```

Time zero starts when you start the `emaneeventservice` program.