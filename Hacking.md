# Introduction #

Also refer to this [Developer's Guide](http://downloads.pf.itd.nrl.navy.mil/docs/core/core-html/devguide.html) section in the CORE manual. The TclTkOverview page was created to collect notes on the GUI code.

Where possible, _follow the coding style used in existing code_. For example, CORE consists of Tcl/Tk scripts from the IMUNES project, C kernel code for FreeBSD, and C userspace code. Please refer to the appropriate style guidelines for the kernel code, for example.

All contributions to the CORE project must adhere to these standards.

# Standards #
**licensing** - CORE is BSD licensed. Only free software having the same or compatible licensing is accepted.

**identifier naming conventions**
  * Python:
    * for the most part, follows the [Python Style Guide](http://www.python.org/dev/peps/pep-0008/)
    * classes are CamelCase with uppercase first letter, example: `class CoreNode():`
    * functions and class methods are all lowercase, words concatenated together, example: `def setposition(self, ...):`
    * variable names are lowercase words, can have underscores;
    * constants are defined in all uppercase, with underscores separating words; defined constants should be used instead of magic numbers, example: `CORE_API_PORT = 4038`
  * Tcl/Tk:
    * function names are CamelCase with a lowercase first letter, example: `proc findFreeIPv6Net { mask } {`
    * variable names are lowercase words, can have underscores; global variables that are not lists or arrays have a `g_` prefix, example: `set node_types "remote"; global g_prefs`
    * constants are defined in all uppercase, with underscores separating words; defined constants should be used instead of magic numbers, example: `set DEFAULT_RANGE 275`
  * C:
    * function names are lowercase words separated by underscores, example: `int receive_file_msg(int, uin8_t *, uint16_t);`
    * variable names are lowercase letters or words with words separated by underscores, example: `int max, err, do_flush;`
    * constants are defined in all uppercase, with underscores separating words; defined constants should be used instead of magic numbers, example: `#define MAX_NODES 32`
    * global variables are used sparingly, named in all lowercase as above, should have a `g_` prefix, example: `struct core_wlan_model *g_models`
    * macros are defined in all caps with words combined together, example: `#define MAC2STR(addr) ...\`

**indentation**
  * Python: **four-space indenting** is used, using spaces and **not tabs**
  * Tcl/Tk: proc declarations are not indented; **four-space indenting** is used, with tabs used to represent each indentation of **eight spaces**; example
```
proc myProc { node } {
....foreach ifc [ifcList $node] {
>-------set peer [peerByIfc $ifc]
```

  * C: lines start with an **eight-space tab indenting** (a tab character, not eight space characters) and each level is indented with a tab character, example:
```
>-------if (do_daemon) {
>------->-------/* do something */
>------->-------if (error_condition)
>------->------->-------exit(1);
```

**column width**

All lines of code must not exceed the standard **80-column width**.

Lengthy string constants can be wrapped in the middle:
```
        wl_log("This is my really long and important message... 79 80"
               "which has been wrapped to fit %d bytes\n",
               len);
```

**comments**

Code should be commented to document its function.

Python comments for the most part, follows the [Python Style Guide](http://www.python.org/dev/peps/pep-0008/). Classes and methods are commented as follows:
```
    def setup(self):
        ''' Client has connected, set up a new connection.
        '''
```

Tcl comments may appear at the end of lines where needed or on separate lines:
```
    # search all nodes for the value
    foreach node $node_list {
        set sum 0; # initialize counter
```

C comments use the slash/asterisk form `/* good */`, not the C++ double slash `// bad`. Comments that are wrapped should repeat the asterisk character on each new line
```
/* here my essay on why this is the fastest
 * algorithm possible. */
```

**expressions**

  * C if statements look like this example. Variables are declared at the start of functions. A space follows the token `if` and braces appear on the same line. Braces are not required if the statement has only one line. Braces and indentation must be consistent within the same if block.
```
        fp = fopen("/dev/urandom", "r");
        if (!fp) {
                seed_with_time = 1;
        } else {
                fgets((char *) &seed, sizeof(seed), fp);
                if (fread(&seed, sizeof(seed), 1, fp) != 1)
                        seed_with_time = 1;
                fclose(fp);
        }
```

**file headers**
  * every source code file needs a block of comments at the top indicating copyright, and usually a short description of the contents of the source file
  * C header files need to contain ifdef protection:
```
/*
 * CORE
 * (c)2009 the Boeing Company
 *
 * widget.h
 *
 * Defines how CORE Widgets are used.
 */
#ifndef _WIDGET_H_
#define _WIDGET_H_
...
#endif /* _WIDGET_H_ */
```

**portability**

Consideration should be given for the code working on different platforms. FreeBSD and Linux are certainly required. Some code, such as the Span daemon, also need to compile and run on Windows.