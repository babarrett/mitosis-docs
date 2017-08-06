## Mitosis Testing Methods

STATUS: Still very early. Working on the test to run and design.

### Units to test

#### Receiver unit
  * **Pre-assembly and flashing the Pro Micro**
  * Pro Micro, USB: Plug into USB cable, recognized flashing software?
  * Pro Micro, flash with QMK works?
  * Pro Micro, USB: Plug into USB cable, recognized by OS as a keyboard?
  * **After components are assembled on to receiver PCB**
  * Check the continuity between the wireless module and the 4-pin header on the PCB.
    * wireless module pin-12 = VCC = header pin-1
    * wireless module pin-31 = SWDIO = header pin-2
    * wireless module pin-32 = SWCLK = header pin-3
    * wireless module pin-1 & pin-11 = GND = header pin-4
  * Wireless module, initial tests can be done without The Pro Micro, just
attach the wireless module to the receiver PCB.
  * Double-check ST_LINK V2 cable connection (in the right order, making contact) to wireless module
  * Wireless module, flashing using OpenOCD works?

#### Keyboard units
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

#### Integration testing
  * With the units all having been tested individually it is time to do the system testing.
  1. Flash the left keyboard half with the production file for the left half.
  2. Flash the right keyboard half with the production file for that half.
  3. Flash the Pro Micro with the production Mitosis QMK file.
  * Insert the batteries in both keyboard halves. Double-check battery polarity.
In the left keyboard half the battery is positive up, and the right half, the
battery is positive down.
  * Plug the receiver unit into the computer (cable), see if all the keys
respond correctly. If so, we're done. If not it's time to trouble-shoot the system.

#### Trouble-shoot the System
  * Wireless module, sends broadcast from keyboard to receiver wireless module?
  * Constant interrupts happening?
  * **After the preceding checks out**
  * Pro Micro, receives messages (serial) from wireless module? (requires test SW for wireless)
  * Wireless module, receives broadcast from keyboard wireless module?


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

The keyboard (each half) will start out in testing mode. It will send 3 bytes of
data at a time to the receiver, with the bit for switch 24 (unused) set. Only if
that bit is set will the receiver treat this as test results.

We'll start with 6 tests, from easiest to pass to hardest. All 6 tests should
fit fine in a single hex file. More tests can be added later. The tests are
described in the following message table.

The format of the 3-byte messages are:
```
    0x800000 || m## - A message number string to display
    0x800000 || k## - A key number (01 to 23) to display
    0x800000 || k99 - End of a list of keys
    0x800000 || vHH - Keyboard sowtware version number (0xHHHH) to display
    0x800000 || _## - Key ## (01-23) pressed
    0x800000 || T## - Key ## (01-23) released
```
The receiver (Pro Micro) outputs the result of each message to the HID system,
and you can use HID-listener to watch the messages.

The "m" messages are looked up in a table and the actual string (given below) is
displayed to the HID.

Receiver-only message:
```
m01 -  Receiver Version info + "\nDo not press any keys yet.\nRunning tests without INTs."
```
Messages from keyboard to Receiver:
```
vHH - Keyboard sowtware version number (0xHHHH) to display.
        Receiver will output to HID: Keyboard software version: 0000 to FFFF
  ==== test #1 ==== Stuck keys without any typing. No sleeping, no interrupts.
m03 - Keys that seem to be down, stuck...
kxx - One message per key number forming a list of key numbers, in this case 01-23 
        (decimal) that are pressed. 
        An example could be:
        01 03 06 22\n
  -or-
m09 - "None" [sucess]

  ==== test #2 ==== Keys that do not register, even though typed. No sleeping, no interrupts.
m12 - You have 8 seconds to press every key, once...
m13 - 8 (These count-down values are printed every 2 seconds
m14 - 6
m15 - 4
m16 - 2
m17 - 0
m20 - Keys recorded as pressed...
kxx - One message per key number forming a list of key numbers, in this case 01-23 
        (decimal) that are pressed. 
        An example could be (k99 represents end of list):
        01 03 06 22 99
  -or-
k99 - (k99 without and kXX before means "none")

  ==== test #3 ==== Turn interupts on for 3 keys, see if they fire without touching them.
m25 - Interrupts enabled for 3 keys, 1,6,11.
  - Enable for some keys that are NOT stuck down.
  - wait 2 seconds, accomulating keypresses -
m06 - Keys that seem to be down, stuck (key down)...
        An example could be (k99 represents end of list):
        01 06 11 99
  -or-
k99 - (k99 without and kXX before means "none")

  ==== test #4 ====
m40 - You have 4 seconds to press *these 3 keys*, once...
k01
k06
k11
m15 - 4
m16 - 2
m17 - 0
m20 - Keys recorded as pressed...
kxx - Key numbers, in this case 01,06,11 that were pressed.
        An example could be (k99 represents end of list):
        01 06 11 99
  -or-
m09 - "None"

  ==== test #5 ==== All interrupts on. See if any keys get recorded as pressed.
m25 - Interrupts enabled for all keys...
    (wait 2 seconds to record/sum pressed keys)
m03 - Keys that seem to be down, stuck...
kxx - One message per key number forming a list of key numbers, in this case 01-23 
        (decimal) that are pressed. 
        An example could be:
        01 03 06 22\n
  -or-
m09 - "None" [success]

  ==== test #6 ==== All interrupts on. See if all keys get recorded as pressed.
m25 - Interrupts enabled for all keys...
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

  ==== test #7 ==== Show key transactions for next XX keys pressed
m25 - Interrupts enabled for keys...
m60 - We will now display the key-down, and key-up events for the next XX keys pressed.
_##
T##
_##
T##
_##
T##
 :

  ==== done ====
m65 - Testing complete. Switching to production keyboard software.
```
