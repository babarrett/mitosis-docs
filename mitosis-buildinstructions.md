## HW ASSEMBLY / the Build process

This is based upon reverse_bias's [build log](https://imgur.com/a/mwTFj#JTzXTCD).

**Safety warnings**
* Ventilation
* Googles, especially for breaking out the squares to make the plate.
* Filter mask

Assemble the parts. See Parts List

### Prepare the base neoprene foam

If you are using a laser cutter for the foam, send the foam out for cutting.

If you are using an Exacto(tm) knife, use a PCB as a template and to trim the foam to size.


### Prepare the plates


**Safety warnings**
* Googles, especially for breaking out the squares to make the plate.
* Filter mask

**Strong suggestion:**

It is suggested you build one side of the keyboard to completion before starting the
second side. This way, if you make a mistake you will only have to repare or
replace one side.


Remove the 23 square switch positions from 2 of the PCBs. The remaining board becomes the
plate. Because of the thin material, especially on the sides, used a desk vice
for these. They are fragile and subject to breakage.

Optionally you can consider (untested) scoring both sides of the tabs you are
about to break with a rolling glass cutter, or (also untested) scoring or
completely cutting with a power tool like a Dremel. Safety equipment is
considered essential if you do this.

The tabs are drilled out to form 'mouse bites'. Insert a pencil into the center
hole of the square and rock the breakout back and forth about 20 times, all the
glass fibers will break, and they just fall out.

If you do it too fast, the soldermask will flake off as well. Don't worry too much
about wrecking these breakouts though, you're just going to throw them away.

There us a little material left, but this is hidden between the case tabs on an
MX switch, so it doesn't need to be completely flush. Use flush cutters to 
knock off any sharp bits though.

As the PCBs are used for the lower connections and the upper plate, the
unpopulated footprints on the top surface need to be covered. Use black mylar
tape, as it matches the texture of the PCB and is very thin. Cut off the
overhang into the switch hole with a knife. 

Electrical tape could work as well.

Set the plates aside. Now we start on adding the electronics to the PCB.


### Prepare the PCBs

**IMPORTANT: Solder the wireless module to the PCB before the 4-pin headers.**

On the bottom of the lower PCB solder the Wireless module. It's a core51822 (B)
from Waveshare. Test to make sure there is no continuity between adjacent pins
on the wireless module.

**IMPORTANT: trim down the pins before dropping the headers into the holes and
soldering from above.**

**IMPORTANT: Make sure the pins on the 4-pin headers, once trimmed to the edge
of the board, are long enough to reach into the ST-LINK V2 programmer.**

Solder a reverse polarity protection FET (SI2302) and a programming header onto
the other side (top) of the board. These are the only electronic components. As
there are 23 switches and 31 GPIOs on the wireless module, there is no need for
a matrix or diodes, The keyboard delivers n-key rollover with a direct GPIO pull-up
connection for each switch.

There is room next to the header for an LED and a resistor. This was designed
into the PCB for initial development and is not being used, It is unknown which
is the cathode pin, or what the resistor value should be, or which pads go to
which device. There is no silkscreening indicating device or orientation. OK to
ignore this.

Repeat on the other half, with programming headers cut flush to the edge of
the board. The programming header ends up mostly hidden, and it's optional anyway.

The space between the upper plate and lower PCB is 3.5mm, more than enough to
fit the 3.2mm coin cell battery. Add some, **but not too much**, solder to the
battery contact pads to fill the gap. Some in the bottom of the plate, and some
on the top (header side) of the PCB.

### Join the PCB and plate with the switches and 2 header pins

We are using 5pin/PCB mount switches here, so there's plenty of coupling between
the boards. Single header pins are added on each side of the battery for
stability. These also carry the battery voltage from the upper plate board to
the lower electronics board.

Insert one switch onto each hole on the upper plate, snapping the switch in place.
Place the plate and switches, switch side down on your work surface.
Insert two (2) straight headers, one on each side of the battery, long end up.

Gently lower the PCB (wireless module side up) onto the switches and two header
pins. Make sure the switches and their leeds are fully inserted.

Once the boards were properly pressed together, solder the switches in.

Solder the header pins on either side of the battery:
* first, cut the top of the header pine flush with the top (plate) board.
* next, solder the top of the battery stabilizers to the plate.
* then turn the keyboard over. Insert a battery between the PCBs and squeeze the
PCBs as you solder the bottom of each stabiliser. the spring of the fiberglass
will hold the battery snugly.

**Pause the work on the keyboard halves. We'll come back to the neoprene
base later.**

### Prepare the Receiver

The receiver is an interface PCB, a Pro Micro, and another identical wireless
module. As all the wireless functionality is handled by the module firmware, it
is completely compatible with QMK using only a custom matrix.c file. 

The surface mount stuff on the receiver is crammed in and tiny. Requires
magnification and tweezers.

**Test the Pro Micro**
* Plug the Pro Micro USB connector into a USB cable attached to the computer.
* Program it using the standard QMK/mitosis hex file. (TODO: true? I use Teensy on my Mac)

There is a button to reset the Pro Micro and reprogram, 
a button for the wireless module to initiate pairing, and an RGB LED for
signalling (Currently using it for layer state and a low battery indicator).

Attach the wireless module to the receiver PCB using the surface mount
"fingers," just like you did on the keyboard halves. Test to make sure there is
no continuity between adjacent pins on the wireless module. (Desolder and try
again if you find unwanted continuity (bridges).

Solder on the small surface mount components to the receiver PCB:
* Solder the two (2) 1206 4.7k resistor arrays side-by-side onto the 16 pads,
close to the USB connector edge of the board, furthest from the wireless module.
There is no wrong orientation, no "Pin 1."
* Solder the LED onto the 4 remaining pads closest to the Reset switch. The pad
with the small silkscreened circle is for pin 1 of the LED. Pin 1 on the LED is
the one where the corner has been clipped off. See
[mouser.com](http://www.mouser.com/ds/2/90/CLVBAFKA-470871.pdf)
* Solder one of the switches to the pairing switch location. Four (4) pads
between to the "P" and the wireless module.
* Solder the 1117 3.3v regulator (SOT223 form factor) onto the 4 pins near the wireless module. 
* Solder the reset switch to the 4 pads next to the "R"
* (optional) Solder a little speaker to pin 5 on the Pro Micro for music mode on the receiver.

You want to solder two 6 pin straight headers into the Pro Micro, at the USB connector end.
* Put the pin headers in a breadboard (long ends down, short ends up) to hold them straight and aligned for
soldering.
* Place the Pro Micro component-side down over the headers. TX0 to 3 on one
side, RAW to A2 on the other.
* solder in place

Attaching the receiver PCB to the Pro Macro
* Test-fit the receiver PCB to the Pro Macro
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
* Use ST-LINK V2 to program the keyboard halves wireless modules
* Use ST-LINK V2 to program the receiver wireless module
