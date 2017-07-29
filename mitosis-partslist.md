## Parts / Bill of materials

* Receiver module
	* 1 Receiver Interface PCB, OSHPark or other PCB house.
	* 1 Pro Micro controller. Leonardo Pro Micro ATmega32U4 5V/16MHz. Replaces ATmega328 Arduino Pro Mini
	* 1 nRF51822 wireless module (see keyboard halves list for full details)
	* 12 Straight 0.1" headers (2 x 6 pins) to connect receiver PCB to Pro Micro
	* 1 1117 3.3v regulator in SOT223
	* 2 1206 (3216 metric) 4.7k resistor array, labeled "472" [Mouser.com](http://www.mouser.com/ProductDetail/Yageo/YC164-JR-074K7L/?qs=sGAEpiMZZMvrmc6UYKmaNXwfiSjkvz6n4e34vrjrWiE%3d)
	* 2 SMD tactile buttons (Reset and Pairing)
	* 4 (optional) Right angle 0.1" header. 4 pins. Used for programming the wireless module on the receiver
	* 1 (optional) LED RGB diffused 4PLCC SMD. Looks like CLVBA-FKA-CAEDH8BBB7A363 or CLVBA-FKA-CA1D181BB7R3R3
should work fine. TODO: Verify

* Keyboard halves
	* 4 Mitosis Keyboard PCBs (2 for plates, 2 for PCB)
	* 2 nRF51822 wireless module (Contains a Nordic Semiconductor
	51822-QFAC1-1513AN SOC chip). It appears that these are single-sourced
	though [Waveshare](http://www.waveshare.com/core51822-b.htm) $7ea + $3 shipping, direct or $11 on Amazon.
	* 2 reverse polarity protection MOS FET (SI2302)
	* 2 CR2032 batteries
	* 2 x 4-pin Right angle 0.1" headers
	* 4 x 1-pin Straight 0.1" headers -- Used on either side of each battery
	* an (optional, low priority) LED can be added to existing pads on the
keyboard halves. It could be used to aid debugging new wireless code.
	  * 1 single, LED SMD.
	  * 1 single, 3.3k resistor SMD.

* Keycaps

    46 1U keycaps, with the following profiles (or for DSA they are all the same):
```
    |Profile| Qty | Description                                |
    +-------+-----+--------------------------------------------+
    | R1    | 10  | Q-P - main alpha keys                      |
    | R3    | 10  | A-; - main alpha keys                      |
    | R4    | 10  | Z-/ - main alpha keys                      |
    | R1    |  8  | Top row of thumb keys                      |
    | R2    |  6  | Bottom row of thumb keys                   |
    | R3    |  2  | Bottom, inverted, row of thumb keys        |
    +-------+-----+--------------------------------------------+
```

* Other
	* USB cable to Micro USB. Short or long, depending on your intended use/placement.
	* 46 Cherry MX compatible key switches
	* 2 Neoprene, adhesive backed, 4x4", 3mm thick. To be trimmed
	* 1 (optional) 3D Print or off-the-shelf case for the Pro Micro receiver
	


### Parts warnings:
There are two versions of the nRF51 board black (wrong) and blue (right). The
green board doesn't physically match the PCB footprint. The correct dimensions
are: 17.0mm x 20.8mm. The bad dimensions are: 15.09mm x 20.07mm.

The angled headers one builder ordered have a couple of mm between the 90 degree angle and
the plastic piece. When he cut these flush with the board, the remaining pin
length was too short to connect the ST-Link wires to. His fix for this was to
pull the plastic part off since the solder is strong enough to hold the pins in
place. It would be better to get an angle header that has the plastic closer to
the bend (but that would make soldering them from the top harder)

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

