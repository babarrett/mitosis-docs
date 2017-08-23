## HW ASSEMBLY / the Build process

This is based upon reverse_bias's [build log](https://imgur.com/a/mwTFj#JTzXTCD).

**Safety warnings**
* Ventilation
* Googles, especially for breaking out the squares to make the plate.
* Filter mask

Assemble the parts, check that they by complete by comparing to the Parts List.

### Prepare the base neoprene foam

If you are using a laser cutter for the foam, send the foam out for cutting.

If you are using an Exacto(tm) knife, use a PCB as a template and to trim the foam to size.
Later we'll finish off the cuts on the inner part of the neoprene.

### Prepare the Receiver

The receiver is an interface PCB, a Pro Micro, and a wireless
module. As all the wireless functionality is handled by the module firmware, it
is completely compatible with QMK using only a custom matrix.c file. 

The surface mount components on the receiver are cramped and tiny. Assembly
likely requires magnification and tweezers.

We will be assembling and testing the receiver first because it is needed to
test the keyboard halves, later.

#### Test the Pro Micro
* Plug the Pro Micro USB connector into a USB cable attached to the computer.
* Program it using the standard QMK/mitosis hex file found in the mitosis/precompiled directory of
the reversebias github repository. https://github.com/reversebias/mitosis/tree/master/precompiled
(TODO: I use the Teensy app on my Mac to do this.)

There is a button to reset the Pro Micro and reprogram, a button for the
wireless module to initiate pairing (not yet implemented July-2017), and an RGB
LED for signalling (Currently using it for layer state and a low battery
indicator).

#### Attach the wireless module
Attach the wireless module to the receiver PCB using the surface mount
"fingers," just like you'll do on the keyboard halves. Test to make sure there
is no continuity between adjacent pins on the wireless module. Desolder and try
again if you find unwanted continuity (bridges).

#### Attach the surface mount components
Solder on the small surface mount components to the receiver PCB:

* Solder the two (2) 1206 4.7k resistor arrays side-by-side onto the 16 pads,
close to the USB connector edge of the board, furthest from the wireless module.
There is no wrong orientation, no "Pin 1."
* Solder the LED onto the 4 remaining pads closest to the resistor arrays and
Reset switch. The pad with the small silkscreened circle is for pin 1 of the
LED. Pin 1 on the LED is the one where the corner has been clipped off. See
[mouser.com](http://www.mouser.com/ds/2/90/CLVBAFKA-470871.pdf)
* Solder one of the switches to the pairing switch location. Four (4) pads
between to the "P" and the wireless module.
* Solder the 1117 3.3v regulator (SOT223 form factor) onto the 4 pins near the
wireless module. 
* Solder the reset switch to the 4 pads next to the "R"
* (optional) Only some group-buys of Mitosis kits come with the wireless module
preprogramed. If your wireless module did not come preprogramed or you think 
there is a chance that you will want to later reprogram the
nRF51822 wireless module on the receiver, for example to support encrypted
communications or dynamic pairing at a later date, you can solder an additional
4-pin, right angle header to the "under-side" of the receiver board. This is the
same side the Pro Micro will be attached to, and the side beneath the wireless
module itself. Note that the hole with the square hole is "pin-1" of the
connector. Also note that you'll always have easy access to this part of the
board, so you can easily add it later. Lastly, you could leave this off and 
connect it, when needed, by holding pins on the programmer to the plated holes
for the few seconds it takes to program it.


**WARNING: Some identical looking ST-LINK V2 programers have their pins in different orders!**
```
| Value | ST-LINK V2 | Receiver |        | Value | ST-LINK V2 | Receiver |
+-------+------------+----------+        +-------+------------+----------+
| 3.3V  | Pin-8      | Pin-1    |        | 3.3V  | Pin-8      | Pin-1    |
| GND   | Pin-6      | Pin-2    |        | SWCLK | Pin-6      | Pin-4    |
| SWDIO | Pin-4      | Pin-3    |        | GND   | Pin-4      | Pin-2    |
| SWCLK | Pin-2      | Pin-4    |        | SWDIO | Pin-2      | Pin-3    |
+-------+------------+----------+        +-------+------------+----------+
```
* (optional) Solder a little speaker to pin 5 on the Pro Micro for music mode on the receiver.

#### Attach the headers to the Pro Micro

You want to solder two 6 pin straight headers into the Pro Micro, at the USB connector end.

* Put the pin headers in a breadboard (long ends down, short ends up) to hold them straight and aligned for
soldering.
* Place the Pro Micro component-side down over the headers. TX0 to 3 on one
side, RAW to A2 on the other.
* solder in place

#### Attaching the receiver PCB to the Pro Micro
* Test-fit the receiver PCB to the Pro Micro
* Cut the through pins flush before soldering to guarantee a smooth solder
joint.
* solder all 12 pins in place

Receiver done. I first programmed the module with OpenOCD for the wireless
functionality. Programming for keymap changes can be done in the normal way for
QMK.


### Test the components
* Insert the Pro Micro USB connector into a USB cable attached to the computer.
Verify that it registers as a USB device.
* Use USB to program the Pro Micro, again. This confirms that you did not break
the Pro Micro while soldering things to it.
* Use ST-LINK V2 to program the receiver wireless module. (Optional if yours is already programmed.)


### Prepare the plates

**Safety warnings**

* Be sure you are wearing googles, especially for breaking out the squares to make the plate.
* Filter mask if you are using a Dremel(TM), or similar rotary grinder to assist in removing
the squares in the "plate" PCBs.

**Strong suggestion:**

It is suggested you build one side of the keyboard to completion before starting the
second side. This way, if you make a mistake you will only have to repare or
replace one side.

Remove the 23 square switch positions from 1 of the PCBs. The that PCB becomes the
plate. Because of the thin material, especially on the sides, used a desk vice
for these. They are fragile and subject to breakage.

Optionally you can consider (untested) scoring both sides of the tabs you are
about to break with a rolling glass cutter, or (also untested) scoring or
completely cutting through the tabs with a power tool like a Dremel. Safety
equipment (googles and mask) is considered essential if you do this.

The tabs are drilled out to form 'mouse bites'. Insert a pencil or end of an
exacto(TM) knife into the center hole of the square and rock the breakout back
and forth about 20 times, all the glass fibers will break, and the square of PCB
material will just fall out.

If you do it too fast, the soldermask may flake off as well. Don't worry too much
about wrecking these breakouts, you're just going to throw them away anyway. You
**do** want to be careful not to brake the remaining "plate."

There is a little material left, but this is hidden between the case tabs on an
MX switch, so it doesn't need to be completely flush. Use flush cutters to 
knock off any sharp bits though.

As the PCBs are used for the lower connections and the upper plate, the
unpopulated footprints on the top surface of the plate need to be covered. Use
black mylar tape, as it matches the texture of the PCB and is very thin. Cut off
the overhang into the switch hole with a knife. Electrical tape could work as
well.

Set the plate aside. Now we start on adding the electronics to the PCB.


### Prepare the PCBs

**IMPORTANT: Solder the wireless module to the PCB before the 4-pin headers used
to flash the wireless nodule.**

There are 3, or optionally 5 components to be soldered on to each keyboard half
PCB that the switches will end up being soldered to. They are as follows:
```
| Part                                | Location |
+-------------------------------------+----------+
| nRF51822 wireless module            |  Bottom  |
| reverse polarity protection MOS FET |  Top     |
| 4-pin Right angle 0.1" header       |  Top     |
| (optional) single, LED SMD.         |  Top     |
| (optional) Resistor SMD for LED.    |  Top     |
|    unknown value, 3.3k may work     |  Top     |
+-------------------------------------+----------+
```

#### Bottom of the PCB - Attach the Wireless Module

Turn the PCB over, bottom-side up.

For the novice, or extra cautious, verify that there is connectivity from one
side of every switch location to a corresponding pin that the wireless module
will be soldered to. Use a multimeter in continuity mode. Use the following
photo to test each switch position:
```
    +-----+
    |photo|
    +-----+
```

TODO: Repeat for the ground connection to the key positions. Faster and easier.

On the bottom of the lower PCB solder the Wireless module. It's a core51822 (B)
from Waveshare. Test to make sure there is no continuity between adjacent pins
on the wireless module.

Repeat the key position test from above to verify the connections are all still
in place.

#### Top of the PCB - Attach the 4-pin Header

Turn the PCB over, top-side up.

**IMPORTANT: trim down the pins before dropping the headers into the holes.**

The short (vertical) ends of the 4 Right angle header are longer than they need
to be. Place the header into the 4 holes near the edge of the board, facing the
edge. The plastic joining piece will not be touching the board. Remove the
header, trim the ends and re-fit it so that the plastic joining piece just 
touches the board.

Push the plastic joining piece as close to the bend as possible. This makes the
horizontal pins as long as possible.

Place the header back in the holes, and solder it in from above.

**IMPORTANT: Test-fit to make sure the pins on the 4-pin headers, once trimmed
to the edge of the board, are long enough to reach into and make contact with
the ST-LINK V2 programmer.**

Trim the 4 horizontal pins flush with the edge of the PCB board.

#### Top of the PCB - Attach the reverse polarity protection 

Solder a reverse polarity protection FET (SI2302) onto the
top side of the board.

As there are 23 switches and 31 GPIOs on the wireless module, there is no need
for a matrix or diodes, The keyboard delivers n-key rollover with a direct GPIO
pull-up connection for each switch.

#### Top of the PCB - Attach the optional LED and resistor

(Optional) There is room next to the header for an LED and a resistor. This was
designed into the PCB for initial development and is not being used in the
"production" software. The pad closest to pin-4 of the programming header is the
anode pin of the LED, the pad closest to pin-3 is the cathode pin. A resistor
goes between the two remaining pads. A value of about 3.3k should work fine.

#### Test your work so far.

1. Temporarily solder a single switch in position on the PCB
2. Attach the ST-LINK programming module to the 4 pins as described TODO: where?
3. Program the wireless module using the instructions TODO: where?


### Repeat on the other keyboard half (PCB)

Test the second board the same way as the first.

### Join the PCB and plate with the switches and 2 header pins

The space between the upper plate and lower PCB is 3.5mm, more than enough to
fit the 3.2mm coin cell battery. Add some, **but not too much**, solder to the
battery contact pads to fill the gap. Some in the bottom of the plate, and some
on the top (header side) of the PCB.

We are using 5pin/PCB mount switches here, so there's plenty of coupling between
the boards. Single header pins are added on each side of the battery for
stability. These also carry the battery voltage from the upper plate board to
the lower electronics board.

Insert one switch onto each hole on the upper plate, snapping the switch in place.
Place the plate and switches, switch side down on your work surface.
Insert two (2) straight headers, one on each side of the battery, long end up.

Gently lower the PCB (wireless module side up) onto the switches and two header
pins. Make sure the switches and their leeds are fully inserted.

Once the boards were properly pressed together, solder the switches in. Start
with the corner switches, for alignment.

#### Solder the header pins on either side of the battery:
* first, cut the top of the header pins flush with the top (plate) board.
* next, solder the top of the battery stabilizers to the plate.
* then turn the keyboard over. Insert a battery between the PCBs and squeeze the
PCBs as you solder the bottom of each stabilizer. The spring of the fiberglass
will hold the battery snugly.

### Finnish off the neoprene bottoms

TODO: add instructions
