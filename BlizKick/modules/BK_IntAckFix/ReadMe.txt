Short:    BlizKick-Module: fixes interrupt acknowledge for 68040/60
Uploader: wepl.iaf@whdload.de (Bert Jahn)
Author:   wepl.iaf@whdload.de (Bert Jahn)
Type:     util/boot
Version:  0.1

Overview
--------

This is a BlizKick (Harry Sintonen) module which fixes the interrupt
acknowledge bug which occurs on fast 68040/60 machines and is (to my
knowledge) present in all kickstart versions (also in piru's exec44).

The patch adds a 'tst.w (_custom+intreqr)' before the 'rte' in the 
Exec function ExitIntr used internally to end interrupts. This makes
sure, that the intreq line has been cleared before quitting the
interrupt handler. And with that avoid obsolete interrupts.

This patch is tested only with a custom build romimage using kick31 +
updates + exec44 using Remus 0.97 on a A4000 CSPPC 060/50.
It should work with all kickstarts >=v37.
It makes no sense to use this on CPU's lower than a 68040 because the
problem to fix will not occure on these systems (although it will not
harm these).

This patch will improve the performance because dummy interrupts will
be avoided. 
I wrote this to find out if this is the reason for bad performance of
my ethernet card. But it seems the xsurf.device driver has other
problems too.
You will probably not notice a performance gain. A good chance to see
a speed increase may be the serial line which causes a lot of
interrupts. But I have not testet that.

Usage
-----

See BlizKick or Remus docs how to use it.


ChkInts
-------

This is a small command line tool which patches the Level-2 interrupt
vector to check if there are interrupts without the appropriate bit
set in the intreq register. It prints the number of all interrupts
occured and the number of interrupts without request. 
You can specify the interval time in ticks (1/50s) for the print out.
The default interval is 10 (0.2s).

To check if you have the interrupt acknowledge bug run ChkInts. The
second number should be always zero. Pressing the keyboard should not
cause interrupts without request.


History
-------

0.1 (03.12.2007)
- initial release


Thanks
------

Harry (Piru) Sintonen for BlizKick and exec44
doobrey for RomSplit and Remus

