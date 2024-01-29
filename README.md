# Neptune 32X/MD2

The Sega Neptune was an unreleased console that would have been a stand-alone 32X.
This project provides a PCB that combines the 32X and Mega Drive/Genesis 2 that fits
in an original Mega Drive/Genesis 2 case or the reimagined Neptune case designed by
DVIZIX.

**NOTE**: In order to use these PCBs successfully you will need to be proficient in:

* Soldering-fine pitched surface mount components.
* In most cases, desoldering said components to obtain these from a donor board.
* Troubleshooting any issues that may arise.

The authors are not obliged to provide support, nor shall they be held responsible
for botched attempts at using these boards. Should you however find a legitimate flaw
in the board's design or have suggestions for improvements, please feel free to
submit an issue.


## Features and Limitations

* Fits original Mega Drive/Genesis 2 or DVIZIX Neptune cases (separate power/reset switch locations are provided for each).
* Modern power circuit by Zaxour (requires 9V PSU rated for at least 2A).
* Integrated switchless region mod using PIC16F630 or compatible programmed with the `switchless.hex` file in this repo ([source here][Switchless]).
* Integrated Mega Amp audio mod as found on Triple Bypass boards.
* Support for Sega Master System games via flash carts or the Sega Power Base via integrated mod by DogP (compatibility not extensively tested).
* Compatible with the original Sega/Mega CD.
* Optional +9V DC to the expansion edge connector (connect the labeled pads with a suitable wire).

* NOT (yet) compatible with flash cart-based Sega/Mega CD implementations such as the Mega EverDrive.
* As the 32X hardware is integrated into the design, there is NO way to disable this.


## PCB Production

Minimum track widths, clearances and via sizes are within the standard
offering of modern PCB fabricators. Gerber files are generated with the relevant
options for both [JLCPCB](https://jlcpcb.com/) and [PCBWay](https://www.pcbway.com/).
Placeholders for the order number are provided so remember to select
the "Specify location" option for this when ordering. If using another service
you may wish to remove or replace this placeholder.

The design is verified to work as a 4-layer PCB with 1 oz outer and 1/2 oz inner layers.
A leaded HASL finish is adequate and ensures compatibility with the original
solder that will inevitably be left behind on salvaged parts. If you plan to use a
Mega CD/Sega CD or anything that uses the edge connector you might want to opt for
an ENIG or other gold finish for this.

For independent testing of the Mega Drive side, you might like to try Chrissy's
experimental bypass boards for IC504 and IC505 in the ``helpers`` directory (more
info below in the build notes). Due to the pitch of these 


## Bill of Materials

For a complete BOM, consult the generated [Interactive BOM][IBOM] or the KiCad
project itself. Parts are numbered according to the following scheme:

| Number  | Description                                        |
|:-------:|:---------------------------------------------------|
| < 500   | Original to Mega Drive/Genesis 2                   |
| 500-599 | Original to 32X (subtract 500 for original number) |
| >= 600  | Additional/replacement parts                       |

It is assumed that most parts will be obtained from donor Mega Drive/Genesis and
32X units. The project was initially based on a VA1 PAL Mega Drive 2 and VA1 PAL
32X. Other Mega Drive/Genesis revisions may be usable if these also use a 315-5660
or compatible ASIC. All 32X revisions should be usable but this has not yet been
confirmed. Note the differences in parts between 32X revisions as documented
in the schematics. It is probably not feasible to list the differences between
all possible revisions but should you work out a solution for a particular donor
board, please let us know so we can include it in this documentation.


## Build Notes

### General

* The ICs on the 32X main board (the "cartridge" part) are glued on making removal
  tricky. The glue can be softened using isopropyl alcohol, so you may want to give
  the board soak in this before starting to avoid having to use too much heat.

* Clean all boards thoroughly with e.g. isopropyl alcohol prior to starting.
  Especially HASL boards from JLC have been known to arrive with some contaminants
  present which prevent reliable solder joints. Tinning pads will expose any such
  issues as the solder will not adhere properly.

### Order of Operations

It is possible to break down parts the build into separate steps to allow for easier
testing and troubleshooting. The following is a suggested order of operations.

* Build the power circuit (all parts in the X800 range) and test this separately.
  It should provide a steady 5V supply even without any load.

* Add the 3.3V regulator (IC510) for the 32X SDRAM and its associated capacitors.
  Check for 3.3V on the regulator output as labeled on the board.

* You may wish to install the PIC for the switchless region mod at this point as
  it should operate independently of the rest of the system. Hold down the reset
  button to cycle through regions as indicated on the RGB LED. If the LED changes
  constantly, check pull-up R701 is installed. The PAL/NTSC and JP/ENG outputs
  should change as appropriate. Verify that a short press of the button generates
  a /WRES pulse.

* If not using the switchless mod, or to bypass it for testing/troubleshooting,
  link the indicated pins and set the desired PAL/NTSC and JP/ENG values using the
  +5V and GND pads on the IC701 footprint. Connect the LED pin of your choice to +5V
  and a single colour LED for LED701.

* At this point it's time to strap in an install all the MD2 components and the
  audio and video parts shared between MD and 32X. This cannot be independently
  tested without bypassing the 32X parts that usually connect to the cartridge slot.
  To this end, you may like to use the bypass boards for IC504 and IC505 (see the
  `helpers` directory). These boards are designed to fit in place of the 32X ICs
  and simply pass the required signals directly to the Mega Drive components.



## Thanks/Credits

  * Zaxour for the power circuit design, support and hardware review.

  * Leo Oliveira, Simon "Aergan" Lock, Chrissy and Dennis for their insights, support
    and assistance during testing.

  * The rest of the Board Folk for their support and general coolness.


## Legal

Copyright ©2024

You are free to produce PCBs based on this project's designs at your own risk
and without limitation, for your own use or for sale and/or repair at a
reasonable price. Attribution is appreciated. The authors are not obliged to
provide support of any kind.

Under no circumstances will the authors be held responsible or liable in any
way for losses, damages or costs resulting from the use of the information
and/or resources of this project.

The resources are provided "as-is" without warranty of any kind, either
expressed or implied, including, but not limited to, the implied warranties
of merchantability and fitness for a particular purpose.

[IBOM]: http://htmlpreview.github.io/?https://raw.githubusercontent.com/Board-Folk/Neptune/master/bom/ibom.html
[Switchless]: https://github.com/atomicretronl/switchless