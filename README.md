# max232\_conv

A dual 5V serial to RS232 converter for use in RC2014 systems

Version 0.1a, July 2023

## What is this?

This is a dual 5V-to-RS232 converter designed for use with the FTDI-style 5V
serial ports found in many RC2014-type retrocomputers.  It allows real RS232
devices to be connected, rather than serial USB converters, and handles RTS/CTS
hardware flow control. It is designed to be used with, and mounted alongside, a
dual SIO serial board, or a motherboard like the SC130 that has two serial
ports. 

You'll need something like this to connect an RC2014 system to a genuine
vintage serial terminal, modem, or printer.

The design is based on the standard MAX232 IC, which contains built-in
voltage converters to generate the +/-12V that RS232 needs. So this
module needs only a single 5V supply.

The two converters in this module are absolutely identical.

The board is shaped to fit the RC2014 40-pin bus but, in fact, it has no
connections to the bus except ground. The power for the module is drawn from
the headers that connect the serial module or motherboard, along with the
serial signals themselves.  In fact, this module doesn't need to be connected
to the bus -- it could just be attached to a chassis using the mounting holes.
It's only bus-mounting for convenience: if there's a spare slot in the
backplane, it's a handy place to mount a board like this, rather trying to find
some other space for it in a ase. When mounted this way, this module connects
to the serial module or motherboard using two short, 6-pin dupot-style
connectors.

So far as I know, all the RC2014 serial boards use the same pin layout for
their headers, so the cable should be 'straight'; that is, pin 1 to pin 1, 2 to
2, and so on.

On the top of the board are two five-pin headers for connecting the RS232
ports, and a traditional DB9. The DB9 is connected in parallel with the 'COM0'
port header. 'COM1' has a 5-pin header, but no DB9, because the standard RC2014
board is not long enough to fit two DB9s.

The 5-pin headers are intended to connect to male DB9s on (say) the back panel
of a system. The specific RS232 connections are labeled on the board itself.

The converters have support for RTS/CTS flow control, provided that the
appropriate pins are wired to the RS232 connection.  However, the SC130 only
has hardware flow control on its COM0 port. This is a limitation of the CPU.
Dual serial I/O boards usually have hardware flow control on both ports.

## Design 

This repository contains a Kicad 6 project, which provdes the circuit schematic
and PCB layout. 

The design has two MAX232 dual transceiver ICs in 16-pin DIP format, ten
capacitors, and various header connections. To use the board you'll also need
two 6-pin dupont-style header cables, and whatever cabling is needed to connect
the RS232 ports. 

All capacitors except decoupling capacitors C9 and C10 are 1uF electrolytic.
16V rating should be fine, but 63V parts fit the board fine. C9 and C10 are
100nF ceramic or similar.

Some published designs for the MAX232 show a further 1uF capacitor in
parallel with the 100nF decoupling capacitors. I have not found this
extra capacitor to be necessary -- but conceivably it might be, 
if the connections to the 5V power supply are long enough.

## Fabrication

I had my board made by JLCPCB, from the zipfile in the `gerbers/' directory. 
The board outline is 50mm x 100mm, which puts it in JLPCB's cheapest
pricing tier -- I paid US$2 for five boards. 

## Assembly

I hope it's self-explanatory. I would recommend sockets for the ICs,
particularly if you can't vouch for their authenticity (see below) and might
end up replacing them. I would recommend soldering the IC sockes first, then
the pin headers, then the backplane connector, then the capacitors, then the
DB9 connector (if used).

If you're mounting this board in an RC2014 backplane, I would suggest using
right-angle headers, as otherwise the connections to the board will get in the
way of nearby boads in the backplane.  If mounting elsewhere, either vertical
or right-angle headers will work fine, according to the layout constraints.

## Notes

There are many 5V-to-RS232 modules on the market, some very cheap.  However,
most of these have no support at all for hardware flow control. 

** Important ** This board provides a _DTE_ interface to the external RS232
device. It does not swap RX/TX or RTS/CTS, as some similar modules do.
Connecting it to a printer or terminal will require a null modem cable, not a
straight cable.

The design uses two Maxim MAX232 chips in 16-pin format. There are many cheap,
nasty imitations of the original Maxim part for sale, particularly on eBay.
These cheap immitations are unreliable, and prone to osciallation and
overheating (I know, because I've tried some myself). If you're going to build
this design, please do yourself a favour and buy authentic MAX232s from a
reputable supplier.

Please note that this module is designed to connect to 'real' RS232 hardware,
not to an emulated, USB-based RS232 adapter. It _may_ work with a PC
USB-to-RS232 adapter: I have some adapters that work perfectly (including flow
control), and some that don't. The problem is that modern PC hardware is just
too fast. Even if a PC USB-to-RS232 device handles hardware flow control, these
devices often have high latencies. That is, if the retrocomputer de-asserts
RTS, to tell the PC to stop sending, the PC might still send a dozen bytes
before it responds. In any event, if you want to connect your retrocomputer to
a PC, there's no need for RS232: an FTDI-style USB adapter should work
perfectly well, and be less complicated.

The MAX232 is specified to work at up to 115,200 baud. I've had no problems
using this module at that speed, with a 3m RS232 cable.

The PCB design provides three mounting holes of 3mm diameter. If using these to
mount the board on a panel, bear in mind the design as it currently stands does
not isolate the mounting holes from the ground backplane. 

There's no significance to the electrolytic capacitors in the board photo being
different colours -- I just ran out of blue ones whilst assembling the board.

The MAX232 chips get a tiny bit warm in use. 

## Legal

`max232\_conv` is an open-source hardware design, released under the
terms of the Creative Commons Share-alike Licence, version 4.0. You
may build and use the design, but the distribution of modifications
is only permitted if the modified designs will also be open-source. There
is, of course, no warranty of any kind.


