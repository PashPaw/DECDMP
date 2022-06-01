# DECDUMP for the PDP-6/10

DECDUMP was the typical Microtape/DECtape bootloader that was written by DEC for the PDP-6 and used for the first Monitor and JOSS-II.  This version was compiled using simh’s PDP-10 KA simulator using MACRO-10 v47 and was largely unaltered from file located (and copied and pasted) from SAILDART.  It has been tested only to the point to boot off of the simulated paper reader in simh’s PDP-6 simulator, and as of writing this README, likely works to load PDP-6 software from DECtape.

The included .bin paper tape image has been compiled for 32kW machines and has a start address of 77600 octal.

The known commands are:

| Command | Description                                                                                                                                                                  |
| ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *n*L    | Load  from DECtape unit *n*.  This appears to be DECtape unit 0 usually.  It can be 1 according to some of the JOSS-II documentation.                                        |
| G       | Start execution of the program off of DECtape and after loading the address in the switch register.    In the case of JOSS-II, this would appear to be SWR address 20 octal. |
| D       | Currently undocumented.  This command shows up in the source code.                                                                                                           |

This is how far I’ve gotten:

![decdmp](decdmp.jpg)

## Compilation instructions (written for TOPS-10 5.03):

- Attach either decdmp.old or decdmp.new to the virtual paper tape reader in Richard Cornwell's KA10 simulator and attach a virtual papertape to the virtual paper tape punch.  For the binaries in the repo, these are *.bin files.

- Run ` pdp10-ka mon503.ini` at a command prompt.

- Sign into PPN [1,2] at the console.

- At the . prompt for TOPS-10, type the following:
  
  ```assign
  assign ptp<cr>
  r pip<cr>
  ```

- PIP should then bring up a * for the prompt.

- Type in the following in PIP:``` dsk:decdmp.mac_ptr:```. PIP should then read the text into a file on the virtual disk image.  Type ^C when done.

- Next, type the following (MACRO will provide a * for a prompt like PIP):
  
  ```
  r macro<cr>
  ptp:_decdmp<cr>
  ```
  
  This should provide a bootable paper tape image for the KA10 or PDP-6 simulators.  It should also produce a listing file as DECDMP.LST.

- Type ^C again and  shut down the simulator as usual with ^E, `set cty stop`, and `quit`.

- For testing, run the PDP-6 simulator and attach the binary to the papertape reader.  Tell it to boot from ptr and use ^E, and then examine the memory contents.

## Special Thanks

To Lars Brinkhoff for assisting me with this and providing an earlier version of DECDMP from SAILDART.

## Todo

- Figure out how to build the DECtape version mentioned in the [1965 Monitor manual](http://bitsavers.org/pdf/dec/pdp6/DEC-6-0-EX-SYS-UM-IP-PRE00_Multiprogramming_System_Manual_1965.pdf).  There appears to be two versions: one for paper tape and one that seems to be self-contained on a DECtape.
- Attempt to make a DECDMP format DECtape.  Any attempt to read a tape so far seems to have crashed the PDP-6 simulator.
