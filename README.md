Enhanced_GDB
============

Development in GDB

[PATCH] Implementation of pipe to pass GDB's command output to the shell.

This patch implements a new gdb command namely, "pipe" to pass gdb
command output to a shell command for further processing. This patch
includes necessary documentation change and test-cases as well.

Command usage: 

pipe <dlim> <gdb-cmd> <dlim><dlim> is a string of arbitrary length containing 
no whitespace and no leading -, acts as a seperator between a GDB command <gdb-cmd> 
and a shell command <shell-cmd>. 
 
Example: 

(gdb) ptype dd_table
type = struct device_driver {
    char *dev_name;
    unsigned long dev_id;
    int (*op)(void *);
    struct policy *dev_policy;
} [1024]
(gdb) pipe | print dd_table | sed 's/}/\n/g' | grep custom_driver | tr ',' '\n'
  {dev_name = 0x8055555 "custom_driver"
  dev_id = 100
  op = 0x0324101
  dev_policy = 0x8012345
(gdb)
 
In the above example the output of the original GDB command could be very much messy.
Use of shell command like grep or sed has improved the readability of the command 
output a lot and hence debugging has become easier. 
 
