# Teensy CAN Bus Monitor

This is a small sketch that is used to monitor CAN Bus activity.

It is essentially a clone of arduino-canbus-monitor by latonia, https://github.com/latonita/arduino-canbus-monitor, modified for use on Teensy 3.x processors.  It uses IFCT to emulate mcp_can which allows it to be used with with CAN breakout boards other than MCP2515.  A version of IFCT which you add to your libraries folder.  For the latest version of IFCT go to https://github.com/tonton81/IFCT.

# Description from the arduino-canbus-monitor by latonia

Can bus monitoring tool based on arduino and can bus shield. Implements CAN ASCII/SLCAN protocol compatible with Lawicel CAN232/CANUSB. Can be used for monitoring low speed CAN (interior can bus of most cars, less than 500kbps). For higher speeds serial port can become a bottleneck in case data density is high. 

NOTE: I have tested this on a Teensy 3.6 at 1,000,000 bps and no problem

# arduino-canbus-monitor [![Build Status](https://api.travis-ci.org/latonita/arduino-canbus-monitor.svg?branch=master)](https://travis-ci.org/latonita/arduino-canbus-monitor) [![Coverity Scan](https://scan.coverity.com/projects/11684/badge.svg)](https://scan.coverity.com/projects/latonita-arduino-canbus-monitor) [![Analytics](https://ga-beacon.appspot.com/UA-99380399-1/welcome-page)](https://github.com/igrigorik/ga-beacon)

CAN BUS monitoring software based on Arduino with Seeduino/ElecFreaks CAN BUS shield based on MCP2515 (Numerous other MCP2515 based CAN BUS modules from ebay and aliexpress work well to).

This software implements CAN ASCII / Serial CAN / SLCAN protocol compatible with Lawicel CAN232/CANUSB. 

For my application it was tested with the UAVcan GUI tool and works without issue

3) [Linux] Please dig into direction of SLCAN/SocketCAN, but start from https://github.com/linux-can/can-utils

This monitor uses CAN BUS library forked from https://github.com/Seeed-Studio/CAN_BUS_Shield.

Copyright (C) 2015,2016 Anton Viktorov <latonita@yandex.ru>

You can buy Anton  a beer if you like the tool :o)   [![Donate](https://www.paypal.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=4JPDVHYWUY3LW)


See protocol definition here http://www.can232.com/docs/can232_v3.pdf and here http://www.can232.com/docs/canusb_manual.pdf

Commands not supported/not implemented:  
- s, W, M, m, U.

Commands modified:
-  S - supports not declared 83.3 rate 
-  F - returns MCP2515 error flags
-  Z - extra Z2 option enables 4 byte timestamp vs standard 2 byte (60000ms max)
  
```
CMD | IMPLEMENTED | SYNTAX               | DESCRIPTION
------------------------------------------------------------------------------------------------------------
'S' |   YES+      |   Sn[CR]               Setup with standard CAN bit-rates where n is 0-8.
    |             |                        S0 10Kbit          S4 125Kbit         S8 1Mbit
    |             |                        S1 20Kbit          S5 250Kbit         S9 83.3Kbit
    |             |                        S2 50Kbit          S6 500Kbit
    |             |                        S3 100Kbit         S7 800Kbit
's' |    -        |   sxxyy[CR]            Setup with BTR0/BTR1 CAN bit-rates where xx and yy is a hex value.
'O' |   YES       |   O[CR]                Open the CAN channel in normal mode (sending & receiving).
'L' |   YES       |   L[CR]                Open the CAN channel in listen only mode (receiving).
'C' |   YES       |   C[CR]                Close the CAN channel.
't' |   YES       |   tiiildd...[CR]       Transmit a standard (11bit) CAN frame.
'T' |   YES       |   Tiiiiiiiildd...[CR]  Transmit an extended (29bit) CAN frame
'r' |   YES       |   riiil[CR]            Transmit an standard RTR (11bit) CAN frame.
'R' |   YES       |   Riiiiiiiil[CR]       Transmit an extended RTR (29bit) CAN frame.
'P' |   YES       |   P[CR]                Poll incomming FIFO for CAN frames (single poll)
'A' |   YES       |   A[CR]                Polls incomming FIFO for CAN frames (all pending frames)
'F' |   YES+      |   F[CR]                Read Status Flags.
'X' |   YES       |   Xn[CR]               Sets Auto Poll/Send ON/OFF for received frames.
'W' |    -        |   Wn[CR]               Filter mode setting. By default CAN232 works in dual filter mode (0) and is backwards compatible with previous CAN232 versions.
'M' |    -        |   Mxxxxxxxx[CR]        Sets Acceptance Code Register (ACn Register of SJA1000). // we use MCP2515, not supported
'm' |    -        |   mxxxxxxxx[CR]        Sets Acceptance Mask Register (AMn Register of SJA1000). // we use MCP2515, not supported
'U' |   YES       |   Un[CR]               Setup UART with a new baud rate where n is 0-6.
'V' |   YES       |   v[CR]                Get Version number of both CAN232 hardware and software
'v' |   YES       |   V[CR]                Get Version number of both CAN232 hardware and software
'N' |   YES       |   N[CR]                Get Serial number of the CAN232.
'Z' |   YES+      |   Zn[CR]               Sets Time Stamp ON/OFF for received frames only. EXTENSION to LAWICEL: Z2 - millis() timestamp w/o standard 60000ms cycle
'Q' |   YES  todo |       Qn[CR]               Auto Startup feature (from power on). 

```
