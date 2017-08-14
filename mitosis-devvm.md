# Creating a VM for Mitosis Development

## Document Status

This document is not yet complete, untested, and needs work. It may be useful or misleading, and I don't know which.
It will be updated as I learn more.

## Definitions
  * Mitosis - A 2-piece wireless, programable (QMK), keyboard that communicates to a wireless "dongle" plugged into a USB port.
  * VM - Virtual machine
  * Host system (or simply host) - 
  * ...

## Install VirtualBox, make a new VM, install Ubuntu:

### Download and Install VirtualBox

* Download the app for your host system here: https://www.virtualbox.org/wiki/Downloads
* Version 5.1.26
* (I'm using the macOS version, any there should work the same. Use the one that will run on your computer.)
    * download
    * open (double-click) the .dmg file
    * install (double-click) the VirtualBox.pkg file, verifies the package. It will take a little while to start up.
    * the installer runs...I selected "Install for all users...; the default installation

### Run and configure VirtualBox

* Run VirtualBox (In Applications folder on macOS, or other)
* Click "New" near top-left of window.
* Name your VM as you like. I'll be using "MitosisDev"
* Select "Type: Linux"
* Select "Version: Ubuntu (64-bit)", Continue
* Select RAM to allocate for the running VM. I'll start with 1542 MB, Continue
* Select "Create a virtual hard disk now", Create
* Selet "VHD" ("VDI" cannot be imported into other VMs later, if you wanted to.), Continue
* Select "Dynamically allocated" (Only uses the disk space it needs.) Continue
* Set value for the HD to 30GB, and the default name & location. Create.
* Network / Adapter 1: Enable, Attached to NAT
* Ports / Serial Ports: Disabled
* Ports / USB: Enable; USB 1.1
* Plug in your ST-LINK V2 programmer. Click the USB and "+" icon. Select the ST-LINK device to attach the programmer to the VM (when running).
* Enasble all tabs for the UI
* OK

### Within VirtualBox, create a VM
* VM Name: MitosisDev
* Disk name: mitosisdevhd
* User: mitosis
* Password: my easy os
* 2GB RAM
* 30GB HD, dynamic
* 64-bit OS
* ...

* Install Ubuntu v16.x into the VM

Get a current Ubuntu installer ISO image

* current version is: 16.04.3 LTS
* go to https://www.ubuntu.com/download/desktop and download the v16.x ISO file.
* Mount the ISO as a CD-ROM for your new VM
* Start up the VM, and boot from the Ubuntu ISO
* Install Ubuntu
* DO NOT mess with the Software & Updates system setting!
* **Use the System Settings to update Ubuntu to the latest software.** (TODO: more)
* Install guest additions, see below for instructions.
* Add Oracle VM VirtualBox Extension Pack, see below for instructions.

## Add the ST-LINK V2 programer to the Ubuntu VM
* Launch VirtualBox
* Select your VM
* Select Settings
* Select Ports, and USB
* Click the "Add" button on the right
* Select the ST-LINK USB device to add that hardware to the VM.
* **EVERYTIME** you open the VM, check the USB icon at the bottom of the window
to make sure the ST-LINK is attached. (Checked) Or, check it in the VirtualBox
settings for that VM under: Settings / Ports / USB / USB Device Filters
  
  
-------------------------------------------

## Install OpenOCD programming software, Nordic SDK and tool chain. 

Based off, but with added detail, 
reversebias README at: https://github.com/reversebias/mitosis

### Nordic SDK:

* Download the Nordic SDK by using a browser to go to [the Nordic page](https://www.nordicsemi.com/eng/nordic/Products/nRF5-SDK/nRF5-SDK-v12-zip/54291)
selecting and downloading the latest SDK (12.3.0 for me). This ends up in your ~/Downloads folder.
* go to the terminal and execute:
```
          cd ~
          unzip ~/Downloads/nRF5_SDK_12.3.0_d7731ad.zip  -d nRF5_SDK_12
          cd nRF5_SDK_12
```
For this to compile the directory nRF5_SDK_12 needs to contain:
```
          .
          ..
          components
          documentation
          examples
          external
          license.txt
          mitosis
          nRF5_SDK_12.3.0_d7731ad
          nRF5x_MDK_8_11_1_IAR.msi
          nRF5x_MDK_8_11_1_Keil4.msi
          svd
```


### Install the required tool chain utilities:
  * Install git, OpenOCD and the gcc-arm compiler by going to the terminal and executing:
```
          sudo apt-get update # make packages upto date
          sudo apt install git
          git --version # display git version
          sudo apt install openocd gcc-arm-none-eabi
```
Update the Nordic makefile (for Linux) to point to TODO: ???
  * Edit the Makefile by going to the terminal and executing:
```
          gedit ~/nRF5_SDK_12/nRF5_SDK_12.3.0_d7731ad/components/toolchain/gcc/Makefile.posix
```
  * Replace something like: 
```
          GNU\_INSTALL\_ROOT := /usr/local/gcc-arm-none-eabi-4\_9-2015q1
```
with:
```
          GNU\_INSTALL_ROOT := /usr/
```
Download (clone) reversebias' Mitosis git repository:
  * Clone reversebias' Mitosis git repository by going to the terminal and executing:
```
          cd ~/nRF5_SDK_12
          git clone https://github.com/reversebias/mitosis
```
  * Install the udev rules (copy from get to the /etc directory):
```
          sudo cp mitosis/49-stlinkv2.rules /etc/udev/rules.d/
```

## Test the ST-LINK configuration and software

* Plug in, or replug in the programmer. (should light up!)

  The programming header pins on the side of the keyboard, from top to bottom:
```
            SWCLK
            SWDIO
            GND
            3.3V
```

It's best to remove the battery during long sessions of debugging, as 
charging non-rechargeable lithium batteries isn't recommended.

Launch a debugging session with:
```
            cd ~/nRF5_SDK_12
            openocd -f mitosis/nrf-stlinkv2.cfg
```
Should:
a. cause the LED on the ST-LINK to illuminate
b. give you an output ending in:
```
            Info : nrf51.cpu: hardware has 4 breakpoints, 2 watchpoints
```
If the board is **not plugged in** (or, plugged in wrong) you will instead receive something like:
```
        Open On-Chip Debugger 0.9.0 (2015-09-02-10:42)
        Licensed under GNU GPL v2
        For bug reports, read
                http://openocd.org/doc/doxygen/bugs.html
        Info : The selected transport took over low-level target control. 
               The results might differ compared to plain JTAG/SWD
        adapter speed: 1000 kHz
        Info : Unable to match requested speed 1000 kHz, using 950 kHz
        Info : Unable to match requested speed 1000 kHz, using 950 kHz
        Info : clock speed 950 kHz
        Info : STLINK v2 JTAG v17 API v2 SWIM v4 VID 0x0483 PID 0x3748
        Info : using stlink api v2
        Info : Target voltage: 3.261457
        Error: init mode failed (unable to connect to the target)
        in procedure 'init' 
        in procedure 'ocd_bouncer'
```
Otherwise you likely have a loose or wrong wire.

## Burn Wireless module -- Mac OS X 

### In Terminal 1:

Navigate to cloned mitosis repo and run.
```
          cd TODO: 
          openocd -f mitosis/nrf-stlink.cfg 
```
Expect something similar to the following response.

        Open On-Chip Debugger 0.10.0
        Licensed under GNU GPL v2
        For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
        Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
        adapter speed: 1000 kHz
        Info : Unable to match requested speed 1000 kHz, using 950 kHz
        Info : Unable to match requested speed 1000 kHz, using 950 kHz
        Info : clock speed 950 kHz
        Info : STLINK v2 JTAG v17 API v2 SWIM v4 VID 0x0483 PID 0x3748
        Info : using stlink api v2
        Info : Target voltage: 3.250331
        Info : nrf51.cpu: hardware has 4 breakpoints, 2 watchpoints


### In Terminal window 2:

telnet localhost 4444
        Trying 127.0.0.1...
        Connected to localhost.
        Escape character is ']'.
        Open On-Chip Debugger

In telnet session run the following commands. Replace the information in [square
brackets] with your system and board-specific information:

        > reset halt
        target halted due to debug-request, current mode: Thread 
        xPSR: 0xc1000000 pc: 0x00012b98 msp: 0x20001c48
        > nrf51 mass_erase
        > flash write_image /Users/[username]/[path]/nRF5_SDK_12/mitosis/precompiled/precompiled-basic-[receiver, left, right].hex
        not enough working area available(requested 32)
        no working area available, falling back to slow memory writes
        wrote 16652 bytes from file /Users/[username]/[path]/nRF5_SDK_12/mitosis/precompiled/precompiled-basic-[receiver, eft, right].hex in 3.201579s (5.079 KiB/s)
        > reset
Flashing wireless units complete. exit everything.

## Burn Wireless module -- Linux

### In Terminal 1:

Navigate to cloned mitosis repo and run.
```
          openocd -f mitosis/nrf-stlink.cfg 
```
Expect something similar to the following response.

        TODO: 

### In Terminal window 2:

telnet localhost 4444

        Trying 127.0.0.1...
        Connected to localhost.
        Escape character is ']'.
        Open On-Chip Debugger

In telnet session run the following commands. Replace the information in [square
brackets] with your system and board-specific information:

        > reset halt
        target halted due to debug-request, current mode: Thread 
        xPSR: 0xc1000000 pc: 0x00012b98 msp: 0x20001c48
        > nrf51 mass_erase
        > flash write_image /Users/[username]/[path]/nRF5_SDK_12/mitosis/precompiled/precompiled-basic-[receiver, left, right].hex
        not enough working area available(requested 32)
        no working area available, falling back to slow memory writes
        wrote 16652 bytes from file /Users/[username]/[path]/nRF5_SDK_12/mitosis/precompiled/precompiled-basic-[receiver, eft, right].hex in 3.201579s (5.079 KiB/s)
        > reset

Flashing wireless units complete. Exit everything.

-------------------------------------------

## Development cycle for Wireless Module
### Recompile
TODO: 

    * Try to download the pre-build binary


### Manual programming

From the factory, these chips need to be erased:

          echo reset halt | telnet localhost 4444
          echo nrf51 mass_erase | telnet localhost 4444

From there, the precompiled binaries can be loaded:

          echo reset halt | telnet localhost 4444
          echo flash write_image `readlink -f precompiled-basic-left.hex` | telnet localhost 4444
          echo reset | telnet localhost 4444

Automatic make and programming scripts

To use the automatic build scripts:

          cd mitosis/mitosis-keyboard-basic
          ./program.sh

An **openocd** session should be running in another terminal, as this script sends commands to it.

-------------------------------------------

## Reducing VM disk space:

After all is done, reduce the VM memory to make sharing easier:

  * Save a snapshot of the VM... just in case
  * Google for: reducing ubuntu disk space
    * Remove Old Kernels (If No Longer Required)
    * Uninstall Apps & Games You Never Use (LibreOffce)
      * sudo apt-get remove package-name1 package-name2
      * sudo apt-get autoremove
    * Clean the APT Cache
      * sudo apt-get clean
    * Use A System Cleaner like BleachBit
      * Preview
      * Clean
    * See where all the space is being used: launch the Disk Usage Analyzer
      * apt-get install duc
      * duc index / -x -p # rebuild database
      * duc ui # show results, graph
      * duc ui /usr/bruce # etc
    * See the 10 largest files on the system with: find / -printf '%s %p\n'| sort -nr | head -10
    * localepurge for removing non-english
    * auto rotate, compress and delete your logs.
      
    
-------------------------------------------

## Guest additions

Copy/Paste between the hos OS and VM is not by default. You can enable it by installing them, then 
enabling copy/paste in the settings for your VM.

1. Launch VirtualBox, start your VM.
2. Mount the ISO image on the guest VM's CD drive by selecting Devices->Insert Guest Additions CD Image. 
This popped up a prompt in my VM to install them.
3.  Once you have installed Guest Additions, then you can enable copy/paste:
  * Start the VM
  * Go to Machine > Settings in the file menu.
  * Go to the General tab, then Advanced.
  * Set the Shared Clipboard setting to Bidirectional.

-------------------------------------------

## Install the extension pack 

Make sure it is the same version as your installed version of VirtualBox!

Oracle VM VirtualBox Extension Pack instructions available here: https://www.youtube.com/watch?v=mwKmxxRbvws

  * Download from: https://www.virtualbox.org/wiki/Downloads
  * Select: VirtualBox 5.1.26 Oracle VM VirtualBox Extension Pack
      * Download: All supported platforms
      * The file downloaded is, for example: Oracle_VM_VirtualBox_Extension_Pack-5.1.26-117224.vbox-extpack
  * Run VirtualBox
  * Make sure no VMs are running
  * Select Extensions "Tab"
  * To the right of the "Extensions Packages" list, use the "Adds new package" drop-down to add the Extension pack.
  * A standard open file dialog is presented, navigate to the downloads folder and select and open the downloaded (.vbox-extpack) file.
  * The dialog informs you the USB 2.0 and USB 3.0 and RDP (Remote desktop) wil be supported.
  * Click "Install." Scroll to the bottom of the license agreement, click "I Agree."
  * You may then have to enter your password to start the install.
  * Select and start your VM.

  * Install Oracle VM VirtualBox Extension pack for Remote Display to work. 

Start VM

