# (Un-Official) Sunhokey 2015 Prusa i3 Marlin 3D Printer Firmware
<img align="right" src="Documentation/Logo/Marlin%20Logo%20GitHub.png" />

 Documentation has moved to [marlinfirmware.org](http://www.marlinfirmware.org).

## About this firmware

Important: This code is intended to follow latest stable branch from official Marlin
firmware, feel free to fork, but be aware I might force push this branch to keep the
Sunhokey patches on top of latest stable Marlin firmware.

 This branch has changes for targeting [Sunhokey 2015 Prusa i3](https://www.3dprintersonlinestore.com/full-acrylic-reprap-prusa-i3-kit). Refer to 
 below changes section for details.

Instructions here assume you are familiar with using your OS command line tools, 
you know how to use git (as you'd be using a git repository), build Arduino projects,
and have somehow managed to already print some usable 3D models on your machine with
stock Sunhokey firmware.

### Firmware backup

Before trying to use this firmware first backup your stock firmware (hopefully you
already tested your machine is working properly after fighting with calibration).

In order to download your firmware install Avrdure (I'm using a Mac and Homebrew). This is how you'd back up your firmware flash code, only adjust your serial port:

```
avrdude -p m2560 -c stk500v2 -P /dev/tty.usbserial-AL00X197 -b 115200 -F -U flash:r:prusa_i3_backup.hex
```

And your EEPROM (persistent data), in case new firmware seems to support it:

```
avrdude -p m2560 -c stk500v2 -P /dev/tty.usbserial-AL00X197 -b 115200 -F -U eeprom:r:prusa_i3_backup.eep
```

In order to confirm the applied setting patches apply 'as is' to your machine, confirm
the Configuration.h patches against your machine reading the configuration. The configuration
would show in Repetier Host when you connect directly to your Prusa i3.

Alternatively, just create a .gcode file with a text editor with the `M503' text on in and and empty line following it. Then just read your machine response. Here was mine (as shown by Repetier Host):

```
< 8:04:47 PM: start
< 8:04:47 PM: echo: External Reset
< 8:04:47 PM: 1.0.0
< 8:04:47 PM: echo: Last Updated: May 26 2015 16:38:13 | Author: (none, default config)
< 8:04:47 PM: Compiled: May 26 2015
< 8:04:47 PM: echo: Free Memory: 3800  PlannerBufferBytes: 1232
< 8:04:47 PM: echo:Hardcoded Default Settings Loaded
< 8:04:47 PM: echo:Steps per unit:
< 8:04:47 PM: echo:  M92 X80.50 Y80.50 Z405.60 E80.50
< 8:04:47 PM: echo:Maximum feedrates (mm/s):
< 8:04:47 PM: echo:  M203 X100.00 Y100.00 Z5.00 E50.00
< 8:04:47 PM: echo:Maximum Acceleration (mm/s2):
< 8:04:47 PM: echo:  M201 X5000 Y5000 Z90 E10000
< 8:04:47 PM: echo:Acceleration: S=acceleration, T=retract acceleration
< 8:04:47 PM: echo:  M204 S950.00 T950.00
< 8:04:47 PM: echo:Advanced variables: S=Min feedrate (mm/s), T=Min travel feedrate (mm/s), B=minimum segment time (ms), X=maximum XY jerk (mm/s),  Z=maximum Z jerk (mm/s),  E=maximum E jerk (mm/s)
< 8:04:47 PM: echo:  M205 S0.00 T0.00 B20000 X20.00 Z0.40 E5.00
< 8:04:47 PM: echo:Home offset (mm):
< 8:04:47 PM: echo:  M206 X0.00 Y0.00 Z0.00
< 8:04:47 PM: echo:PID settings:
< 8:04:47 PM: echo:   M301 P22.20 I1.08 D114.00
< 8:04:48 PM: https://www.sunhokey.com Machine:Prusa I3
```

So stock firmware was 1.0.0 as displayed above, built on May 26th 2015 (just before
shipping!).

## Useful links

 * [RepRap Marlin wiki page](http://reprap.org/wiki/Marlin)

 * [Hiboson's Sunhokey Prusa i3 videos](https://youtu.be/OwwfR-UDEFc). There are few
differences on the machine he builds compared to May/2015 Sunhokey's. Anyway, the 
videos shipped with the machine skipped some important parts like how to mount the
control board (anyway, get some PCB spacers for this) or how to attach the Y axis belt.

 * [Thingiverse Sunhokey 3D printer owners](http://www.thingiverse.com/groups/sunhokey-3d-printer-owners). Keep an eye on the improvements other guys are doing.

### Some tips

From my build experience (start of Jun/2015), first 3d printer contact actually:

* Sunhokey videos are hard to follow for correct pieces orientation, take a good look
to shape features.

* Missing MKS v1.1 board mounting instructions, check Hibobon's videos and forums. GIYF.

* It's likely your aluminum bed would be bent, if you find a way to avoid a glass bed 
please let me know. Initially I'm using a two dollars frame glass from Home Depot (+6 USD
for a glass cutter because I could not get my desired), anyway, using glass is really
tricky, there are some good ideas on Thingiverse if you want to avoid holding clips.

* My extruder motor gear was about 3.5mm off (inside position), this was causing the 
filament to be released during printing.

* Use stock firmware to get you started, I'm sure that if anything gets changed the 
Sunhokey guys would adapt it, googling around, seems customer support is not bad.

* The shipped SD card is not compattible (speed, timing, etc) with the stock firmware,
I had to use an 'old 4GB Sandisk' I had (speed 4).

* Get a extension cord with a switch, keep the switch near you when testing to shutdown
the unit quickly before anything breaks (e.g. had you missed to connect any axis stop
switch?).

* IMPORTANT: SD card is really convenient, but something I notticed is that stopping a 
print without first pausing it would leave the unit in a non recoverable state (motion into out of limits). Both pause or stop won't shutdown the printing immediately though (might take
2~20 seconds).

* Get Gym mats before attempting to level your printer, this is to dampen the conducted 
noise from your printer, respect your relatives (and neighbors if you live in an apartment).

* Leveling is so damn tricky!!! This don't even attempt to print if your bed is not flat
and leveled (use a metal ruler in perpendicular size position to 'see' your deformation).

* Printing is slow, simple stuff takes couple of hours, more below. Be sure to have 
time before you get started.

* Use latest 'best' slicer, there are frequent slic3r and Cura release. Sunhokey settings
seem non optimal for slic3r with outdated repetier on Mac.  Download latest Cura and adapt
the settings from Sunhokey's configuration snapshots for Repetier, this will pay off.

* Start with PLA filament. It sticks nicely to a 40 ~ 50 C glass bed and to blue tape.

* Cura is awesome, start with 0.25mm layers (or maybe Repetier/Slic3r on Mac OS X is 
really bad, I don't know). Cura you can quicly show a printing estimate, add multiple
parts (automatically positioned), and get decent results without many tweaks 
(use suggested defaults when in doubt, e.g. brims).

* Autodesk Meshmixer is a nice .obj to .stl conveter.

## Over Upstream Marlin Changes

### Jun/8th/2015

Changes done on top of official 1.0.2 stable branch.

Using settings from M503 gcode response (which actually match to those ones uploaded
to forums for Sunhokey 2015 Prusa i3 machines.

 * MKS v1.1, (RAMPS 1.4 + Arduino ATMEGA2560)

 * Single extruder

 * Temperature thermistor sensors enabled for both extruder and bed

 * Horizontal size limited extra 5mm to account for some glass bed platform restrictions.
   While the shipped bed is aluminum it is not flat enough.

 * Stepper settigs matching reported configuration, along other forum posted settings
   (e.g. REPRAP_DISCOUNT_SMART_CONTROLLER defined, inverted Y direction, but EEPROM 
   storage not enabled).

## Contact

Refer to main Marlin firmware for core project contact information.

## Credits

The current Marlin dev team consists of:

 - Scott Lahteine [@thinkyhead] - English
 - Andreas Hardtung [@AnHardt] - Deutsch, English
 - [@Wurstnase] - Deutsch, English
 - [@fmalpartida] - English, Spanish
 - [@CONSULitAS] - Deutsch, English
 - [@maverikou]
 - Chris Palmer [@nophead]
 - [@paclema]
 - [@epatel]
 - Erik van der Zalm [@ErikZalm]
 - David Braam [@daid]
 - Bernhard Kubicek [@bkubicek]

More features have been added by:
  - Lampmaker,
  - Bradley Feldman,
  - and others...

## License

Marlin is published under the [GPL license](/Documentation/COPYING.md) because We believe in open development.
Do not use this code in products (3D printers, CNC etc) that are closed source or are crippled by a patent.

[![Flattr this git repo](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software)
