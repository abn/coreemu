This page lists planned development milestones.

Update March 2014: note that this page is quite outdated

# (Archive) Plans for 2012 #

## Robustness and Usability Enhancements ##
  * Testing framework and exception system
    * improved support for raising CEL exceptions
    * kernel/system tests
  * Service configuration
    * improved node service customization
    * storing/applying frequently-used configurations
  * Further develop Python scripting examples and documentation
    * clearer tutorial on writing CORE Python scripts
  * Greater control of interface/link configuration
    * install physical interface into namespace
    * expose tc queuing disciplines for power users

## NRL Network Modeling Framework (NMF) Integration ##
  * Goal is to support a well-defined experiment workflow
  * Read and write XML file formats
  * Re-use of NMF tools/scripts where applicable
  * Use Motion Planning Framework (MPF) tools for node mobility
    * currently the user supplies their own ns-2/scengen file
  * provide SDT 3D output
    * one click from 2D GUI to launch 3D GUI
  * Physical Nodes
    * improved managing, assigning, monitoring
    * EMANE support for physical nodes
    * physical interfaces

## Co-Emulation/Simulation Integration ##
  * Xen
    * generalize and merge xen branch into trunk
      * support non-ISO-based filesystems
      * support node services, EMANE
  * EMANE
    * save/load model presets (e.g. RF-PIPE "SATCOM" preset)
    * interoperability with new models/versions
    * improve health monitoring and control mechanisms
  * ns-3
    * integrate node location
    * GUI-based configuration and control of ns-3
      * purely ns-3 simulated nodes
    * update CORE to have channel reference devices

# 4.4 #
Released on 9/25/12. CORE 4.4 addresses these items from the above list:
  * Service configuration
    * improved node service customization
    * storing/applying frequently-used configurations
  * Further develop Python scripting examples and documentation
    * clearer tutorial on writing CORE Python scripts
  * Goal is to support a well-defined experiment workflow
  * generalize and merge xen branch into trunk
  * interoperability with new models/versions
  * improve health monitoring and control mechanisms


# 4.5 #

We plan to organize the milestones listed above into specific future release versions, along with planned release dates.