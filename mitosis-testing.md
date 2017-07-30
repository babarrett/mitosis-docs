## Mitosis Testing Methods

### Components(?) to test

* Receiver unit
  * Pro Micro, USB: Recognized by OS? 
  * Pro Micro, flashing with QMK works?
  * Pro Micro, receives messages (serial) from wireless module?
  * Wireless module, receives broadcast from keyboard wireless module?

* Keyboard unit
  * Switch continuity, for + side
  * Switch continuity, for GND side
  * No solder bridges on the wireless chip
  * No opens (pin to pad) on the wireless chip
  * Pins 2, 4, 6, 8 (SWCLK, SWDIO, GND, VCC) of ST-LINK connector have
continuity to pins: 32, 31, 1, 12
  * Wireless module: Recognized by ST-LINK V2 and OpenOCD? 
  * Wireless module, can flash with ST-LINK V2 and OpenOCD? 
  * Wireless module, sends broadcast from keyboard to receiver wireless module?
  * Others? 

### What are Likely Error modes

* Constant interrupts happening? 
* No interrupts being received? 
* Stuck key. 
* No response to any key.
* No response to key.
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
    * LED lights for 0.25 sec when map pf pressed keys changes
3. Allow interrupts only on three keys: 1,6,11. The rest of the software is
normal so pressing any of these gets through. These are all "normal" displayable
keys. (A-Z).

Only enable int on 3 keys: 1,11,21. See if others are making constant interrupts.




On "start up" (if we can tell) send state of all keys, then send all zeros so we
know what's stuck, if any. 
For debug don't send wireless more than every 1/2 second. 
More robust code. Switch to poll if stuck or const int's 
