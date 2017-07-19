# MITOSIS 2017 -- Information, parts, build instructions

This document has been created using available resources on Reddit.com, Geekhack.org, and other
web sources. The intent is to gather all useful and available information to make building
and programming the Mitosis wireless keyboard as easy as possible.

TODO: 
* Check name and clock speed on Pro Micro
* ATmega32U4 processor?
* battery life? , but it would still last years in standby, akin to the battery shelf life.
* Add photos, especially annotated close-ups of boards for soldering small parts.
* Break into multiple files?

--------------------------------------------------------------------------------------
## OVERVIEW
### Design Goals
* Open-source design with all schematics, PCB layouts, etc. available
* Wireless design
* caseless thin form factor (no wrist rests)
* Both halves of the board communicate wirelessly with the receiver. 
* Very low power, long battery life
* Keyboard processor is kept in deep sleep as much as possible
* Using key interrupts to help accomplish this
* scanning a keyboard matrix would mean the processor is on "all the time" so a matrix is not used
* each key is, therefore, wired to the processor directly, and no diodes are needed.
* Small PCB, 10x10cm for reduced production costs in small quantities
* The same PCBs are used for the left and right side of the keyboard
* The same PCBs are used as the "plate" for the keyboard
* low latency
* the receiver, and therefore keyboard is QMK software compatible
* encrypted communication from keyboard to receiver TODO: True? Not done yet? Check source.
* The Mitosis firmware is included into the default QMK firmware set.
  * intelligent layers 
  * macros
  * handles all the standard QMK functions

### Physical characteristics
* The keyboard contains 3 parts: Left keyboard half, right keyboard half, and receiver that plugs into 
the computer.
* The receiver and Pro Micro assembly needs to be connected to the computer USB port via a cable.
* 23 keys on each half, 46 total.
* Key columns and rows are staggered for 15 keys per side, although closer to 
ortholinear then "traditional" keyboards.
* Ten keys per side (the thumb keys) are arranged in a 2 x 8 ortholinear grid.
* The plate does not support switch top removal.
* There are 6 "layers" to the keyboard. Starting at the top (key caps) and moving to the desk they are:
```
    | Layer | Description                                      |
    +-------+--------------------------------------------------+
    | 1     | Keycaps                                          |
    | 2     | Switches (extends down to PCB layer)             |
    | 3     | Plate                                            |
    | 4     | PCB                                              |
    | 5     | Wireless controller attached to underside of PCB |
    | 6     | Neoprene foam attached to underside of PCB       |
    +-------+--------------------------------------------------+
```

### Glossary available
At the QMK project [glossary](https://github.com/qmk/qmk_firmware/blob/docs/docs/glossary.md).

### User Operation
Once built and tested, plug in the receiver to a USB cable connected to a USB
port on the computer and type. 
There's no need to "wake up" the keyboard halves by pressing a key. The keyboard
is always on.

Unless you somehow substantially change the design of the keyboard halves there is no need to reprogram the 
wireless modules on either keyboard half, or the receiver.

If you want to change the layout or function of the keys you can recompile with
QMK and reprogram the Pro Micro, just like many other keyboards.

#### Changing the batteries
The little + and - icons next to the battery pads refer to the pad on the same
side rather than indicate the battery orientation.

In the left keyboard half the battery is positive up, and the right, the battery is positive down!

The CR2032 batteries are not rechargeable.

--------------------------------------------------------------------------------------
## BACKGROUND

### Links:
  * [git markdown format](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#links) for editing this doc.
  * Creator: [reverse_bias (reddedit)](https://www.reddit.com/user/reverse_bias)
  * Builds logs and info:
    * Original [photo build log](http://imgur.com/a/mwTFj)
  * Group buys and Intrest Checks:
  TODO:  double check, especially flashquark
    * [GB, by PhenixFire]( https://www.reddit.com/r/mechmarket/comments/6bp4s0/gb_mitosis_diy_kit_3_days_to_go/) - Parts, unassembled. Wireless modules pre-flashed. May 17, 2017.
    * [GB by kaybeerry](https://www.reddit.com/r/MechanicalKeyboards/comments/6l99re/mitosis_partial_builds/) July 4, 2017. Selling maybe 2-10 units.
    * [IC by Touareg3@flashquark.com](https://www.reddit.com/r/MechanicalKeyboards/comments/6lpuy8/ic_mitosis_wireless_split_ergonomic_keyboard/) 
on Reddit. July 6, 2017. Assembled keyboard in an acrylic case. PCBs modified with screw holes for the case.
    * [IC by flashquark](https://geekhack.org/index.php?topic=90442.msg2453492#msg2453492) on Geekhack. July 6, 2017.
    * [GB by Touareg3](https://www.reddit.com/r/MechanicalKeyboards/comments/6m56id/gb_mitosis_wireless_split_ergonomic_keyboard_gb/)@flashquark.com July 9, 2017
    * [GB more info by Touareg3](https://www.reddit.com/r/MechanicalKeyboards/comments/6mm3ke/gb_mitosis_wireless_split_ergonomic_keyboard_with/)@flashquark.com July 11, 2017.

 * Latest [code+readme](https://github.com/reversebias/mitosis)
    * Github of kbd halves and receiver moduals: https://github.com/reversebias/mitosis
  * [OpenOCD](http://openocd.org/), On-Chip Designer
  * Programmer USB to wireless moduals:
    * Search [ebay](http://www.ebay.com/sch/i.html?_from=R40&_sacat=0&_nkw=st+link+v2&_blrs=spell_check) 
for ST-Link V2
    * Search [AliExpress](https://www.aliexpress.com/wholesale?catId=0&initiative_id=SB_20170515155207&SearchText=st+link+v2)
for ST-Link V2

--------------------------------------------------------------------------------------
## Details

### Electrical
* Keyboard:
  * Each keyboard half runs off a single CR2032 (3 volt, lithium) battery
  * There are 4 pins available on a right-angle header between the Plate and PCB
for reprogramming the keyboard wireless modules
  * One nRF51822 wireless module on each half.
  
* Receiver module:
  * Pro Micro, 24 pin. Leonardo Pro Micro ATmega32U4 5V/16MHz.
  * One nRF51822 wireless module
  * Receiver interface PCB (attaches between the Pro Micro and a wireless module).
  * The receiver interface PCB has 4 holes (on 0.1" centers) for a header to program the receiver wireless module.
  * The receiver module is powered by the USB port.
  * The receiver interface PCB has space for a button to reset the Pro Micro and reprogram, 
more space for a button for the wireless module to initiate pairing, and a place
for an RGB LED for signalling.
  * On the Pro Micro there are 12 unused pins.
  * On the receiver wireless module there are 23 pins are available. See [schematics](http://imgur.com/DWFeZvP).

### Software details
The wireless firmware in the 2 keyboard halves is nearly identical, but different (The firmware program "knows" 
which half it's written for.)

Mitosis is NOT using Bluetooth protocol to communicate, even though 
the modules support it. It is using a 
protocol called Gazell, which supports all the frequency hopping for
coexistence, pairing and encryption benefits of Bluetooth while allowing for a
no-compromise split design. 

The keyboard just waits in low power sleep state until a switch is
depressed, or released. It consumes around 3uA in standby, which would 
give 7.5 years standby life out of a standard CR2032. In reality though, 
batteries have an internal self discharge which limits their life.

The receiver module has two micro controllers, the ATmega32U4 on the Pro Micro
to handle the USB, and the nRF51822 in the wireless module to handle the
wireless reception. The Pro Micro can be programmed over USB like any Pro Micro
based QMK keyboard, but the nRF51822 on the receiver wireless needs to be
programmed with the ST LINK V2 programmer, like the keyboard halves.

As all the wireless functionality is handled by the wireless module firmware.
The Pro Micro is compatible with QMK using only a custom matrix.c file.

The Pro Micro has a bootloader, BUT to enter it one has to short GND and RST two
times (tough one can solder a button to it, which is pictured in the build
thread.)

If the Pro Micro comes with a bootloader, you can program it over USB, without
any kind of external programmer needed.

### Firmware
from kaybeerry via /r/MechanicalKeyboards

Be sure to disable some other features if you're going to enable macros and
music. (ROM capacity limitation.)

I am still using the precompiled versions of the nRF51 chips but will try
putting custom code on them later in the week so I can use the pairing feature.

If you use the stock nRF51 firmware, it doesn't require pairing, the addresses
are hard coded.


### The Design: 

TODO: Are these up to date? especially hav reverse\_bias's github moved into QMK?
  * Main [starter link](https://www.reddit.com/r/MechanicalKeyboards/comments/66588f/wireless_split_qmk_mitosis/) 
  * Design materials
    * [PCB manufacturing files](https://github.com/reversebias/mitosis-hardware/tree/master/gerbers)
    * [PCB design files](https://github.com/reversebias/mitosis-hardware/tree/master/altium-mitosis)
    * [PDF Schematics](https://github.com/reversebias/mitosis-hardware/tree/master/schematics)
    * [Lasercutting files](https://github.com/reversebias/mitosis-hardware/tree/master/cnc)
    * [Wireless firmware](https://github.com/reversebias/mitosis)
    * [Mitosis QMK source](https://github.com/qmk/qmk_firmware/tree/master/keyboards/mitosis), now merged QMK
    * [Parts list with suppliers](https://github.com/reversebias/mitosis-hardware/tree/master/bom)

Schmatics can be read, can be imported, displayed and edited with KiCAD. TODO: How to import and save?


-----------------------------------------------------------------------------------------
## Parts / Bill of materials
    TODO: INSERT HERE

--------------------------------------------------------------------------------------
## Tools, ancillary materials required
* Soldering iron suitable for SMT soldering
* Solder paste
* Solder
* Multimeter (continuity tester)
* Exacto(tm) knife
* ST-Link V2 (or clone) to program wireless modules
* tweezers (for SMT work)
* Black mylar tape, or electrical tape to cover top of plates and wireless module.
* magnification
* De-soldering equipment, in case something goes wrong

--------------------------------------------------------------------------------------
## SW DEVELOPMENT

TODO: Complete for macOS Pro Micro and wireless dev.

(reverse_bias) I'll set up a git repo that you can clone into
the extracted Nordic SDK, and then build from there. But I'll also upload some
.hex files if you don't need to modify the wireless. I think most people only
want keymap changes anyway.

reverse_bias: I program/debug with a ST-LINK V2 and OpenOCD.

All the guides for programming the Bluetooth chip are done in Linux. Booting
from a USB drive is probably the easiest and safest option. (Bruce says if you
want to run Linux on macOS maybe use Docker, if someone has created a volume for
this.)

Looks like there macOS programs that will program via the ST-LINK V2 also.
Mac software install for the ST-LINK programmer: http://macappstore.org/stlink/

  (brew install stlink)

(runninghack) Do I need to solder 4 wires on the receiver and connect the
ST-Link V2 to them? I just found there are also 4 holes on the receiver. 

(reverse_bias) Ah. I didn't actually bother to solder to mine, as I was only
programming once. I just held the four pins against the sides of the holes for
the 15 seconds it took to program.



--------------------------------------------------------------------------------------
## HW DEVELOPMENT

TODO: Complete with KiCAD or similar

--------------------------------------------------------------------------------------
## HW ASSEMBLY / the Build process
    TODO: INSERT HERE

### Finishing the neoprene bases

Once the testing is complete...

The neoprene has a tiny bit of give, such that any particles on the desk are
absorbed, but does not compress when you try and squash it into the desk. It's a
little tacky which stops it sliding around, and dampens key noise very well.

Run a sharpie along the board edges here, to colour them black to match the
surfaces.

The neoprene bases lift the board 3mm off the desk. These were lasercut out of
adhesive neoprene, like the underside of a mousepad. This provides 1mm of
clearance for the switch pins and the wireless module. What other keyboard has
full size MX switches only 1mm from the desk? :)

Remove the adhesive backing, stick down the middle first, lining up the cutout 
with the central switch, then slowly worked my way towards the edges keeping it centered.

### Add the keycaps

I bought a 108 key blank PBT set of keycaps. The keyboard only uses 46 1U keys
though.

Map of the profile I used for each key. Grey keys are what I home on, hence the
flipped R3 under my thumbs.



### Conclusion
Fibreglass PCBs are strong, and with the close coupling of the two layers, the
keyboard has almost zero flex when twisted. And since the entire bottom PCB is
supported with dense foam, the only board flex when typing is the desk itself. 

This is the keymap I've settled on for the moment. It's based on the Malt layout
(E under left thumb), and uses the tri-layer feature of QMK for the
function+shift layer. I wanted a single alternative layer for
symbols/numpad/cursor keys. Doesn't matter that I can't reach the page and
volume keys from the home position, as I only use them while browsing. I'm
pretty happy with 50wpm after only 2 months. :)


### Notes

I have a basic temperature controlled iron, Hakko 936 clone. The secret
ingredient is lots of flux, and a youtube search for drag soldering.

Maybe just add "rubber feet" instead of neoprene.

More heat is rarely ever the answer for SMD. If anything, more heat makes the
flux burn off faster, making your work time shorter. I would say that faster
applied lower heat is the actual solution here. I generally solder at 300C, with
a 3.2mm chisel tip like this (much bigger than 0.65mm or 0.5mm pitch pins that
are common in SMD). Using a smaller conical tip to solder smaller things is a
false assumption that too many people make, all it does is limit the rate at
which heat can get all the way to the tip (a problem which you already
identified). A nice thick tip that can get the joint up to 300c in a second,
with some flux cored solder or liquid flux, and just let surface tension work
it's magic.

### Build problems -- things to watch out for

The resistor array can be oriented any direction

the only way to be sure the nRF51 is soldered on correctly is to put a
multimeter in continuity mode and start poking adjacent pins. If it beeps, get
that solder, flux, sucker, and try again.

-------------------------------------------
## Future design thoughts

Q: Is it possible to add a wireless numpad? Using PIPE_NUMBER=2? 
How many devices can run at the same time with the receiver?

A: The only limitation to the number of devices is the Gazell limit of 8. Adding
another wireless keyboard will require simple changes to the wireless module
firmware. PIPE_NUMBER=0, and 1 are already in use. You will also need to have
the ProMicro expect these new pipe communications, and expand it's mental map of
those new key positions. We're only starting with 46 keys, so adding 2 more wireless 
units would bring the total to 96 keys. Easily done.
  
Q: Is it possible to add more keys or columns (with a redesigned PCB)?

A: Yes, though some software changes would be required too. reverse_bias says: I
would say that 29 is trivially easy, 31 is possible if I make a version of the
software that doesn't use the LF crystal. 26-27 would be around the practical
limit for a reversible design like Mitosis. ergomacro notes that adding 1 more
key (24) looks trivial in the software. We're already sending 3 bytes (23 bits)
to represent the state of the keys. 24 keys would still fit in 3 bytes. Going
beyond that requires a fourth byte for the extra keys. Still, not a lot of work.

Q: Any ideas about "tenting?"

A: I think it may be worth trying a pair of these
[stands](https://www.amazon.com/gp/product/B00HHEAMXC/ref=oh_aui_detailpage_o02_s00?ie=UTF8&psc=1).

--------------------------------------------------------------------------------------
## Software needed
* [OpenOCD](http://openocd.org/) To flash wireless
* QMK?? to flash controller?
* ??? to compile QMK
* There is a docker in QMK

--------------------------------------------------------------------------------------
## Resources

* reverse bias's [list of suppliers](https://github.com/reversebias/mitosis-hardware/tree/master/bom).
* The receiver interface PCB can be made by OSHpark.
* The processor on the "wireless module" card is from [Nordic Semiconductor](https://www.nordicsemi.com/eng/Products/Bluetooth-low-energy/nRF51822)
* Nordic Semiconductor also has an SDK and libraries you can compile into your wireless module. [Dev tools and software](https://www.nordicsemi.com/eng/Products/Bluetooth-low-energy/nRF51822).
* 46 switches, PCB Mount, 5 pin. (2 wires, 2 alignment pins, 1 large center pin)
      46 * $0.29 Gateron key switches (blue tac 55, *brown tac 45, *clear lin 35, red lin 45):
      https://techkeys.us/collections/accessories/products/gateron-keyboard-switches
      Reverse\_bias used blues.
* Waveshare has a [Development Wiki](http://www.waveshare.com/wiki/Core51822_(B)).
* 121 1u DSA PBT keycaps for $36 from [Aliexpress](https://www.aliexpress.com/item/white-and-grey-dsa-keycaps-pbt-blank-keycaps-for-wried-mechanical-gaming-keyboard/32793784962.html?spm=2114.01010208.3.10.Ip97XM&ws_ab_test=searchweb0_0,searchweb201602_4_10152_10065_10151_10068_436_10136_10137_10157_10060_10138_10155_10062_10156_10154_10056_10055_10054_10059_100031_10099_10103_10102_10096_10147_10052_10053_10107_10050_10142_10051_10171_10084_10083_10080_10082_10081_10110_10111_10112_10113_10114_10181_10183_10182_10185_10033_10032_10078_10079_10077_10073_10070_10123,searchweb201603_1,ppcSwitch_4&btsid=bde190d2-b208-4e07-a433-5491b2c23ca8&algo_expid=4d53a13e-348f-4808-8ed7-6c76b6d1b1f8-1&algo_pvid=4d53a13e-348f-4808-8ed7-6c76b6d1b1f8).
Enough for 2 keyboards, and some left over. Or for $54 you hsve enough keycaps for 3.

