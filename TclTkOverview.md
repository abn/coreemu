# GUI #
The CORE GUI uses Tcl/Tk 8.5. This page has a brief guide to the source files. The GUI can be found in the source tarball under the `core/gui/` directory.

# Tcl/Tk files #

**`core`**: this is a shell script that launches the GUI. First it sets some environment variables relating to the CORE directories and then launches `wish8.5 core.tcl`.

**`core.tcl`**: main entry point for Tcl/Tk code. This file uses `source` to include all of the other tcl source files. Some important global variables are set here. Preferences and config files are loaded (some procs from filemgmt.tcl). The file `initgui.tcl` is sourced, and initializes more global variables, builds the menus, and sets up the main canvas window. Control is passed to `proc setOperMode` from `exec.tcl` to build the GUI toolbar and menus.

**`initgui.tcl`**: see `core.tcl` above; this initializes the main GUI window.

**`editor.tcl`**: this is the largest Tcl source file. It has procs for building various configuration dialogs for editing typologies.

**`api.tcl`**: the Tcl/Tk implementation of the CORE API, for communicating with the Python CORE daemon back-end.

**`cfgparse.tcl`**: imn file loading and saving.

**`nodecfg.tcl`**: procs for manipulating the Tcl representation of node and link objects.

**`exec.tcl`**: historically this contained shell commands to instantiate virtual machines on the host. Now the Python back-end does all of the node and link creation on the host. There are some procs for switching between edit/execute toolbars and `monitor_loop` is a thread that runs during execute mode to update the CPU/memory usage display.

**`wlan.tcl`**: procs devoted to wireless networks. When the WLAN configuration dialog is invoked, `editor.tcl` will call procs here. Historically, the range calculation and mobility scripting was supported here.

**`widget.tcl`**: run-time widget support, for interacting with a running emulation.

**`mobility.tcl`**: run-time tools, such as the Two Node and Run Tools.

**`traffic.tcl`**: for configuring MGEN-based traffic flows for nodes.

**`wlanscript.tcl`**: for parsing and running a ns-2 scengen format mobility script.

**`util.tcl`**: miscellaneous utility functions.

# Debugger #
Choose _Debugger..._ from the _Tools_ menu for a text entry for executing arbitrary Tcl code. This is a great tool for testing changes you may be making to certain procedures.

Here are some code snippets you might try in the debugger (cut and paste one at a time):
```
puts $node_list
puts $link_list
getNodeCoords n3
hideNode n4
setGuiTitle "hello world"
```