## Mitosis Testing Methods

STATUS: Still very early.

### Units to test

* Receiver unit
  * **Pre-assembly and flashing the Pro Micro**
  * Pro Micro, USB: Plug into USB cable, recognized by OS?
  * Pro Micro, flashing with QMK works?
  * **After components are assembled on to receiver PCB**
  * Check the continuity between the wireless module and the 4-pin header on the PCB.
  * Wireless module, initial tests can be done without The Pro Micro, just
attach the wireless module to the receiver PCB.
  * Double-check ST_LINK V2 cable connection (in the right order, making contact) to wireless modual
  * Wireless module, flashing using OpenOCD works?
  * **After the preceding checks out**
  * Pro Micro, receives messages (serial) from wireless module? (requires test SW for wireless)
  * Wireless module, receives broadcast from keyboard wireless module?

* Keyboard units
  * **Pre-assembly**
  * Switch continuity, for + side of switches
  * Switch continuity, for GND side
  * **After components are assembled**
  * No solder bridges on the wireless chip
  * No opens (pin to pad) on the wireless chip
  * Pins 2, 4, 6, 8 (SWCLK, SWDIO, GND, VCC) of the 4-pin ST-LINK header have
continuity to pins: 32, 31, 1, 12
  * **Flashing**
  * Remove batteries while programming, or when connected to ST-LINK. (ST-LINK will power it.)
  * Double-check ST_LINK V2 cable connection (in the right order, making contact) to wireless module
  * Wireless module: Recognized by ST-LINK V2 and OpenOCD?
  * Wireless module, can flash with ST-LINK V2 and OpenOCD?
  * **After the preceding checks out**
  * Remove the ST-LINK connection

* Integration testing
  * With the units all having been tested individually it is time to do the system testing.
  1. Flash the left keyboard half with the production file for the left half.
  2. Flash the right keyboard half with the production file for that half.
  3. Flash the Pro Micro with the production Mitosis QMK file.
  * Insert the batteries in both keyboard halves. Double-check battery polarity.
In the left keyboard half the battery is positive up, and the right half, the
battery is positive down.
  * Plug the receiver unit into the computer (cable), see if all the keys
respond correctly. If so, we're done. If not it's time to trouble-shoot the system.
  * Wireless module, sends broadcast from keyboard to receiver wireless module?
* Constant interrupts happening?


### Keyboard testing details

### Receiver testing details

### Integration Troubleshooting

#### What are the likely problems?

* No interrupts being received?
* Stuck key.
* No response to any key.
* No response to specific keys.
* Key output is different than expected.
* Multiple keypresses sent for one actual keypress (AAAA sent on A)
* More than one key sent on a single keypress (A+S sent on A)
* No wireless broadcast.


### PCB Continuity Tests

1. Validate wireless module pads to switch-position continuity
2. Validate ground to switch-position continuity
3. Validate lack of continuity between adjacent wireless module pads
4. After attaching wireless module, validate lack of continuity between adjacent pins

### Wireless Radio Tests

1. Prove wireless sending works, no interrupts.

### Key Status Tests

1. Report all "down" keys at power-up. No interrupts. Poll once. Shows "stuck" keys.
2. Add LED to kbds for additional state reporting, such as:
    * Count number of switches down
    * Blink on wireless send
    * Blink on int
    * LED ON when awake, off when in deep sleep
    * LED lights for 0.25 sec when map of pressed keys changes
3. Allow interrupts only on three keys: 1,6,11. The rest of the software is
normal so pressing any of these gets through. These are all "normal" displayable
keys. (A-Z).

Only enable int on 3 keys: 1,11,21. See if others are making constant interrupts.







## Test code design

#### Keyboard communication to the receiver

The keyboard (half) will start out in testing mode. It will send 3 bytes of data
at a time to the receiver, with the bit for switch 24 (unused). 

We'll start with 6 tests, from easiest to pass to hardest. More tests can be
added later. The tests are described in the following message table.

The format of the 3-byte messages are:
```
    m## - A message number string to display
    k## - A key number (01 to 23) to display
```
The receiver (Pro Micro) outputs the result of each message to the HID system,
and you can use HID-listener to watch them.

The "m" messages are looked up in a table and the actual string (given below) is
displayed to the HID.

Messages from keyboard to Receiver:
```
m01 -  Version info + "Please do not press any keys yet.\nRunning tests without INTs."
  ==== test #1 ====
m03 - Keys that seem to be down, stuck...
kxx - List of key numbers, in this case 01-23 that are pressed.
  -or-
m09 - "None" [sucess]

  ==== test #2 ====
m12 - You have 8 seconds to press every key, once...
m13 - 8
m14 - 6
m15 - 4
m16 - 2
m17 - 0
m20 - Keys recorded as pressed...
kxx - List of key numbers, in this case 01-23 (we hope) that were pressed.
  -or-
m09 - "None"

  ==== test #3 ====
m25 - Interrupts enabled for keys...
  - Enable for some keys that are NOT stuck down. Example: 1,6,11.
k01
k06
k11
  - wait 2 seconds, accomulating keypresses -
m06 - Keys that seem to be down, stuck...
kxx - List of key numbers, in this case 01,06,11 or none that are pressed.
  -or-
m09 - "None" [success]

  ==== test #4 ====
m40 - You have 4 seconds to press *those 3 keys*, once...
k01
k06
k11
m15 - 4
m16 - 2
m17 - 0
m20 - Keys recorded as pressed...
kxx - Key numbers, in this case 01,06,11 that were pressed.
  -or-
m09 - "None"

  ==== test #5 ====

m25 - Interrupts enabled for keys...
m12 - You have 8 seconds to press every key, once...
m13 - 8
m14 - 6
m15 - 4
m16 - 2
m17 - 0
m20 - Keys recorded as pressed...
kxx - List of key numbers, in this case 01-23 (we hope) that were pressed.
  -or-
m09 - "None" [success]

  ==== test #6 ====
m25 - Interrupts enabled for keys...
m60 - We will now display the key-down, and key-up events for the next XX keys pressed.
kxx
m62 - down
kxx
m63 - UP

  ==== done ====
m65 - Testing complete. Switching to production keyboard software.
```
