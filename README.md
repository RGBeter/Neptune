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

* Fits original Mega Drive 2 (PAL), Genesis 2 or DVIZIX Neptune cases. Separate
  power/reset switch locations are provided for each (see the `stl` directory for
  files with which to 3D print spacers for the buttons if using the DVIZIX case).
* NOT compatible with the Japanese Mega Drive 2 case due to this using a sliding
  power switch with a different footprint to that of the push button style.
* Modern power circuit by Zaxour (requires 9V PSU rated for at least 1A).
* Integrated switchless region mod using PIC16F630 or compatible programmed with
  the `switchless_xxxx.hex` files in this repo ([source here][Switchless]). Use
  the file that matches your LED's pinout.
* Integrated "Triple Bypass" mod for improved RGB and audio.
* Provisional support for Sega Master System games via integrated mod by DogP
  (compatibility not extensively tested).
* Compatible with the original Sega/Mega CD.\*
* Not compatible with flash cart-based Sega/Mega CD implementations such as the Mega EverDrive Pro.
* As the 32X hardware is integrated into the design, there is NO way to disable this.
* No mono audio output.

\* The model 2 Sega CD/Mega CD uses the +9V pin on the expansion connector to power
itself on along with the attached console. This board does not however provide 9V
to this pin. Testing has shown that the CD hardware will also power on with only 5V
on this pin. The suggested action is to therefore connect the +5V and +9V pads near
B26 and B28 with a wire link. We're not aware of any add-ons that require 9V on the
expansion connector. If you do however really need it, there is another +9V point
near the power jack. This is however not switched along with the console and will
remain powered as long as there is power to the jack. You could rig up something with
a transistor to make 9V switch on along with the console.


## PCB Production

Minimum track widths, clearances and via sizes are within the standard
offering of modern PCB fabricators. Gerber files are generated with the relevant
options for [JLCPCB](https://jlcpcb.com/). The latest revision is also available
as a shared project at PCBWay. Placeholders for the order number are provided so
remember to select the "Specify location" option for this when ordering. If using
another service you may wish to remove or replace this placeholder.

The design is verified to work as a 4-layer PCB with 1 oz outer and 1/2 oz inner
layers. The intended layer order is Front, In1, In2, Back. A leaded HASL finish is
adequate and ensures compatibility with the original solder that will inevitably be
left behind on salvaged parts. If you plan to use a Mega CD/Sega CD or anything that
uses the edge connector you might want to opt for an ENIG or other gold finish for
this. Chamfering of the expansion edge connector is recommended.

For independent testing of the Mega Drive side, you might like to try Chrissy's
experimental bypass boards for IC504 and IC505 in the ``helpers`` directory (more
info below in the build notes).


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
32X. [Other Mega Drive/Genesis][Revisions] revisions may be usable if these also
use a 315-5660 or compatible ASIC. All 32X revisions should be usable but this has
not yet been confirmed. Note the differences in parts between MD and 32X
revisions/regions as documented in the schematics. It is probably not feasible to
list the differences between all possible revisions but should you work out a
solution for a particular donor board, please let us know so we can include it in
this documentation.

Parts marked "PAL" on the board are supposedly only used on PAL Mega Drives. Parts
marked "VA0" and "VA1" are for those revisions of 32X only. However, should your
donor hardware have any of these parts it would seem wise to transfer them to the
new board regardless of the original region/revision.

For later 32X VA0 main boards, pay particular attention to `D699`. If your donor 32X
has a THT diode for `/VRES` on the main board you will need to install this or an SMD
equivalent at this location. The footprint is however bridged, as on other revisions
there is no diode and this is simply a direct connection. If installing the diode
you will therefore need to cut the trace between the pads. The accompanying capacitor
may be installed at location `C695`.

All SMD passives on the board have 0805 or larger footprints for easier soldering.
Inductor footprints are 1210 but you can of course use narrower such as 1206.
If reusing MD2 passives, these should therefore fit well. Many 32X passives are
smaller but should still fit. If you plan to reuse them, do note that particularly
the smaller capacitors are prone to losing their metal end caps and, being so small,
this is not always immediately apparent. If you see solder balling up instead of
flowing nicely, this may well be the problem.


## Build Notes

### General

* Before disassembling the donor consoles, test these thoroughly using the "Mars
  Checker" diagnostic ROM. Please note that running this from some flash carts
  (particular EverDrive models or Mega SD) may yield unreliable results. Even on
  known-good original hardware, you may be presented with a red screen when booting
  or test runs may/may not succeed seemingly at random. For this reason, it's
  recommended to run the tests several times. If you are unable to boot the
  diagnostics using your particular flash cart, consider a simpler single-image
  (i.e. no menu) one or something EPROM-based.

* The ICs on at least one side of the 32X main board (the "cartridge" part) are glued
  on making removal tricky. The glue can be softened using isopropyl alcohol, so you
  may want to give the board soak in this before starting to avoid having to use too
  much heat. Low-melt solder will also help in this respect.

* Clean all boards thoroughly with e.g. isopropyl alcohol prior to starting.
  Especially HASL boards from JLC have been known to arrive with some contaminants
  present which prevent reliable solder joints. You may even like to tin the SMD
  pads and clean up with braid/wick to ensure that solder will adhere properly.

* The Triple Bypass mod features a low pass filter that can be enabled or disabled by
  fitting `R966` or `R965` respectively. As the LPF has both advantages and
  disadvantages, the choice is left to the builder. When using the provided PCB
  assembly files, the LPF is enabled by default. Otherwise, you must fit one of these
  resistors; the enable pin may not be left floating. Enabling the LPF may improve
  Mega Drive image quality at the expense of artefacts in 32X graphics. Disabling it
  may be preferable if you are using a video scaler. YMMV, so do what works for your
  particular setup.


### Order of Operations

It is possible to break down parts the build into separate steps to allow for easier
testing and troubleshooting. The following is a suggested order of operations.

* Build the power circuit (all parts in the X800 range) and test this separately.
  It should provide a steady 5V supply even without any load.

* Add the 3.3V regulator (IC510) for the 32X SDRAM and its associated capacitors.
  Check for 3.3V on the regulator output as labeled on the board.

* You may wish to install the PIC for the switchless region mod at this point as
  it should operate independently of the rest of the system. Install also the LED,
  passives in the X700 range and a temporary ~10K pull-up on `/WRES` (pin 11).
  R702 limits current through the LED and therefore determines its brightness.
  The value is based on the original MD2 LED, so you may prefer to use a higher
  value when using a modern LED, which tend to be brighter for the same current.
  If your LED has protruding "lugs" instead of just straight pins, it's recommended
  to trim these off if possible before installation to avoid damaging the board.
  Hold down the reset button to cycle through regions as indicated on the RGB LED.
  If the LED changes constantly, check pull-up `R701` is installed. The PAL/NTSC
  and JP/ENG outputs should change as appropriate. Verify that a short press of
  the button generates a `/WRES` pulse.

* If not using the switchless mod, or to bypass it for testing/troubleshooting,
  link the indicated pins and set the desired PAL/NTSC and JP/ENG values using the
  +5V and GND pads on the `IC701` footprint. Connect the LED pin of your choice to
  +5V and a single colour LED for `LED701`.

* At this point it's time to strap in and install all the MD2 components and the
  audio and video parts shared between MD and 32X. This cannot be independently
  tested without bypassing the 32X parts that usually connect to the cartridge slot.
  To this end, you may like to use the bypass boards for IC504 and IC505 (see the
  `helpers` directory). These boards are designed to fit in place of the 32X ICs
  and simply pass the required signals directly to the Mega Drive components. If
  using the bypass boards, you will also need to temporarily connect the /YS and
  /AS signals to the cartridge slot. /YS has a labeled test point for this near
  the connector. /AS can be tapped from the labeled pad on `R75`. Remove these
  connections before installing the 32X hardware.

* For a fully assembled board WITHOUT any cables connected there should be around
  350 Ohms between VCC and ground. If you measure something wildly different this
  may indicate a short, an incorrectly installed IC or similar. With the video
  cable connected the resistance will drop to around 150 Ohms.


## Thanks/Credits

  * Zaxour for the power circuit design, invaluable support and hardware review.

  * Chrissy (@chris_jh) for testing, support and work on making pre-assembled PCBs
    possible.

  * Leo Oliveira, Simon "Aergan" Lock, Ian (@grandoldian) and Dennis (@PointerFunction)
    for their insights, support and assistance during testing.

  * The rest of the Board Folk for their support and general coolness.


## Legal

Copyright ©2024

You are free to produce PCBs using this project's designs at your own risk, for
your own use or for sale and/or repair at a reasonable price. Attribution is
appreciated. Publishing files for PCB production (such as, but not limited to,
Gerber files) for financial compensation (sales commission, credit, etc.) on for
example "shared project"-style websites or similar is prohibited.

Derivative works must be released under similar Open Source terms such that all
sources are publicly available. The original authors' names and copyright statements
may not be removed.

The authors are not obliged to provide support of any kind. Under no circumstances
shall the authors be held responsible or liable in any way for losses, damages or
costs resulting from the use of the information and/or resources of this project.

The resources are provided "as-is" without warranty of any kind, either
expressed or implied, including, but not limited to, the implied warranties
of merchantability and fitness for a particular purpose.

[IBOM]: http://htmlpreview.github.io/?https://raw.githubusercontent.com/Board-Folk/Neptune/master/bom/ibom.html
[Switchless]: https://github.com/atomicretronl/switchless
[Revisions]: https://segaretro.org/Sega_Mega_Drive/Hardware_revisions
