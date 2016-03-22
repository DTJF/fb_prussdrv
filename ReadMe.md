FreeBASIC PrussDrv Kit
======================

Welcome to *fb_prussdrv*, a fork of the package am335x_pru_package
containing adaptions for usage with FreeBAISC (FB) programming
language. It's designed to control the Programmable Realtime Unit
SubSystem (= PRUSS) in ARM CPUs by FB source code (ie. on [Beaglebone
Black or White hardware](http://www.beaglebone.org) ).

This package contains

- The customized PRU assemble source code (in folder *util*s) containing
  FB output capability (new option -y)

- FB example code to test (in folder *examples*)

- FB header files (in folder *include*), translated from C source

- The binaries (in folder *bin*) of
  - the library libprussdrv,
  - the device tree overlay and
  - the (LINUX-arm7hf) pasm assembler

The libprussdrv library supports

- loading firmware to the PRUSS
- exchanging data between host and PRUSS
- starting the firmware on the PRUSS


Licence
=======

Copyright &copy; 2015-2016 by DTJF

The source code of this bundle is free software; you can redistribute
it and/or modify it under the terms of the GNU General Public License
version 3 as published by the Free Software Foundation.

The program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
General Public License for more details.

You should have received a copy of the GNU General Public License along
with this package; if not, write to the Free Software Foundation, Inc.,
51 Franklin Street, Fifth Floor, Boston, MA 02110- 1301, USA. For
further details please refer to:
http://www.gnu.org/licenses/gpl-3.0.html


Installation
============

You need the FreeBASIC compiler to make use of this package. See
http://www.freebasic.net/forum/viewtopic.php?f=5&t=23355 for details
and installtion instructions.

Then you can download and extract this package to your Beaglebone and
add the fb_prussdrv extension by executing those commands (in the
directory where you want to download the package)

~~~{.sh}
wget https://github.com/DTJF/fb_prussdrv/archive/master.zip
unzip master.zip
cd fb_prussdrv-master/
sudo su
cp bin/pasm /usr/local/bin
cp bin/libprussdrv.* /usr/local/lib
ldconfig
mkdir /usr/include/freebasic/BBB
cp include/* /usr/include/freebasic/BBB
exit
~~~

When you already installed am335x_pru_package, you can omit the
libprussdrv part, but must override the pasm binary:

~~~{.sh}
wget https://github.com/DTJF/fb_prussdrv/archive/master.zip
unzip master.zip
cd fb_prussdrv-master/
sudo su
cp bin/pasm /usr/bin
mkdir /usr/include/freebasic/BBB
cp include/* /usr/include/freebasic/BBB
exit
~~~


pasm Test
---------

In order to test the PRU assembler, type

~~~{.sh}
pasm
~~~

and you should see output like (note the new option `y  - Create
'FreeBasic ...`)

~~~{.sh}
$ pasm


PRU Assembler Version 0.84
Copyright (C) 2005-2013 by Texas Instruments Inc.

Usage: pasm [-V#EBbcmLldz] [-Dname=value] [-Cname] InFile [OutFileBase]

    V# - Specify core version (V0,V1,V2,V3). (Default is V1)
    E  - Assemble for big endian core
    B  - Create big endian binary output (*.bib)
    b  - Create little endian binary output (*.bin)
    c  - Create 'C array' binary output (*_bin.h)
    m  - Create 'image' binary output (*.img)
    L  - Create annotated source file style listing (*.txt)
    l  - Create raw listing file (*.lst)
    d  - Create pView debug file (*.dbg)
    y  - Create 'FreeBasic array' binary output (*.bi)
    z  - Enable debug messages

    D  - Set equate 'name' to 1 using '-Dname', or to any
         value using '-Dname=value'
    C  - Name the C array in 'C array' binary output
         to 'name' using '-Cname'
~~~


Header Test
-----------

In order to test the header installation and to compile the example
source code execute

~~~{.sh}
cd examples
fbc -w all user_leds.bas
fbc -w all PRU_memAccessPRUDataRam.bas
~~~

When running the first example, you should see output like

~~~{.sh}
$ sudo ./user_leds
	INFO: Starting ./user_leds example.
	INFO: Executing example.
	INFO: example running, press any key.
~~~

and the user LEDs should light up and down, one after the other (system
interupts may interfere with the example control of the LEDs). Press
any key to quit the example.

Running the second example you should see output like

~~~{.sh}
$ sudo ./PRU_memAccessPRUDataRam
	INFO: Starting ./PRU_memAccessPRUDataRam example.
	INFO: Initializing example.
	INFO: Executing example.
	INFO: Waiting for HALT command.
	INFO: PRU completed transfer.
INFO: Example executed succesfully.
~~~

and no further action on the board.

You're ready to use the PRUSS from FB source code now.

Happy coding, share your results!
