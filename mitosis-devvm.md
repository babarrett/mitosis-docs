# Creating a VM for Mitosis Development

## Document Status

This document is in it's infancy. Incomplete, untested, and unreliable.

## Definitions
  * Mitosis - A 2-piece wireless, programable (QMK), keyboard that communicates to a wireless "dongle" plugged into a USB port.
  * VM - Virtual machine
  * Host system (or simply host) - 
  * ...

## Overview:
* Install VirtualBox
  * Download the app for your host system here: https://www.virtualbox.org/wiki/Downloads
  * Run the installer
    
* Within VirtualBox, create a VM
  * VM Name: MitosisDev
  * Disk name: mitosisdevhd
  * User: mitosis
  * Password: my easy os
  * 2GB RAM
  * 30GB HD, dynamic
  * 64-bit OS
  * ...

* Install Ubuntu v16.x into the VM
  * go to https://www.ubuntu.com/download/desktop and download the v16.x ISO file.
  * Install Ubuntu
  * DO NOT mess with the Software & Updates system setting!
  * **Use the System Settings to update Ubuntu to the latest software.** (TODO: more)
  * Install guest additions, see below for instructions.
  * Add Oracle VM VirtualBox Extension Pack, see below for instructions.

* Add the ST-LINK V2 programer to the VM
  * Launch VirtualBox
  * Select your VM
  * Select Settings
  * Select Ports, and USB
  * Click the "Add" button on the right
  * Select the ST-LINK USB device to add that hardware to the VM.
  * Open the VM, check the USB icon at the bottom of the window to make sure the ST-LINK is attached.
  
  
-------------------------------------------
Install OpenOCD programming software, Nordic SDK and tool chain. Based off, but with added detail, 
reversebias README at: https://github.com/reversebias/mitosis

Nordic SDK:
  * Download the Nordic SDK by using a browser to go to [the Nordic page](https://www.nordicsemi.com/eng/nordic/Products/nRF5-SDK/nRF5-SDK-v12-zip/54291)
selecting and downloading the latest SDK (12.3.0 for me). This ends up in your ~/Downloads folder.
  * go to the terminal and execute:
```
    unzip nRF5_SDK_12.3.0_d7731ad.zip  -d nRF5_SDK_12
    cd nRF5_SDK_12
```
Install the required tool chain utilities:
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
If the board is **not plugged in** (or, plugged in wrong) you will instead reveive something like:
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


## Development cycle for Wireless Module

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

An openocd session should be running in another terminal, as this script sends commands to it.


-------------------------------------------
Some is Obsolete(?) below here. Others need to be incorporated.
-------------------------------------------
    
    
  * Download from within the Linux VM
    * the link to the tarball is on [this page](https://sourceforge.net/projects/openocd/files/openocd/0.9.0/).
    * find the link here, near the top of the page: Looking for the latest version? 
[Download openocd-0.10.0.tar.bz2 (4.8 MB)](https://sourceforge.net/projects/openocd/files/latest/download?source=files)
	  * The link (for 0.10.0.tar.bz2) is here: 
  * Download Linux version from: 
  * Make a directory: /...
  * Move to:
  * Unzip
    

Download the N. SDK
  * Download from: 
    

Git Mitosis repository
  * Make sure length is OK
  * Try to download the pre-build binary
    
Get compiler & development tools

Recompile
  * Try to download the pre-build binary







Install VirtualBox

    * https://www.virtualbox.org/wiki/Downloads
    * Version 5.1.26
    * (I'm using the macOS version, any there should work the same. Use the one that will run on your computer.)
        * download
        * open (double-click) the .dmg file
        * install (double-click) the VirtualBox.pkg file, verifies the package. It will take a little while to start up.
        * the installer runs...I selected "Install for all users...; the default installation

Get a current Ubuntu installer ISO image

    * current version is: 16.04.3 LTS
    * go to https://www.ubuntu.com/download/desktop and download

Run and configure VirtualBox

    * Run VirtualBox (In Applications folder on macOS)
    * Click "New" near top-left of window.
    * Name your VM as you like. I'll be using "Mitosis-Dev-Ubuntu_v16"
    * Select "Type: Linux"
    * Select "Version: Ubuntu (64-bit)"
    * Continue
    * Select RAM to allocate for the running VM. I'll start with 1542 MB
    * Continue
    * Select "Create a virtual hard disk now"
    * Create
    * Selet "VHD" ("VDI" cannot be imported into other VMs later, if you wanted to.)
    * Continue
    * Select "Dynamically allocated" (Only uses the disk space it needs)
    * Continue
    * Set value for the HD to 30GB, and the default name & location. Create.
    * If you want to remote access into your VM with "" Go to Settings:
        * --add after experimentation --
        * Display ico at top / Remote display "Tab"; enable server, Port 3389, auth method: Nul??; Timeout 5000, no multiple connections.
        * 
    * Network / Adapter 1: Enable, Attached to NAT
    * Ports / Serial Ports: Disabled
    * Ports / USB: Enable; USB 1.1
    * Plug in your ST-LINK V2 programmer. Click the USB and "+" icon. Select the ST-LINK device to attach the programmer to the VM (when running).
    * Enasble all tabs for the UI
    * OK


-------------------------------------------

## Guest additions

Copy/Paste between the hos OS and VM is not by default. You can enable it by installing them, then 
enabling copy/paste in the settings for your VM.

1. Launch VirtuialBox, start your VM.
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
  * A standard open file dialog is presented, navigate to the downloads folder and select and open the downoaded (.vbox-extpack) file.
  * The dialog informs you the USB 2.0 and USB 3.0 and RDP (Remote desktop) wil be supported.
  * Click "Install." Scroll to the bottom of the license agreement, click "I Agree."
  * You may then have to enter your password to start the install.
  * Select and start your VM.

  * Install Oracle VM VirtualBox Extension pack for Remote Display to work. 

Start VM

