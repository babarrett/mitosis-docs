# Creating a VM for Mitosis Development

## Document Status

This document is in it's infancy. Incomplete, untested, and unreliable.

## Definitions
  * Mitosis - A 2-piece wireless, programable (QMK), keyboard that communicates to a wireless "dongle" plugged into a USB port.
  * VM - Virtual machine
  * Host system (or simply host) - 
    

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
  * ...

Install Ubuntu v16.x into the VM
  * go to https://www.ubuntu.com/download/desktop and download
  * mount the ISO image on he guest VM's CD drive
  * Install Ubuntu
  * Update Ubuntu software
  * Add Oracle VM VirtualBox Extension Pack
  * DO NOT mess with the Software & Updates system setting!

Add the ST-LINK V2 programer to the VM
  * Launch VirtualBox
  * Select your VM
  * Select Settings
  * Select Ports, and USB
  * Click the "Add" button on the right
  * Select the ST-LINK USB device to add that hardware to the VM.
  * Open the VM, check the USB icon at the bottom of the window to make sure the ST-LINK is attached.
  
  
-------------------------------------------
Install OpenOCD programming software, Nordic SDK and tool chain. See reversebias README at:
https://github.com/reversebias/mitosis

Follow the instructions from there, noting the following:
  * Download the Nordic SDK by using a browser to go to [the Nordic page](https://www.nordicsemi.com/eng/nordic/Products/nRF5-SDK/nRF5-SDK-v12-zip/54291)
selecting and downloading the latest SDK (12.3.0 for me). This ends up in your ~/Downloads folder.
  * go to the terminal and execute:
```
    unzip nRF5_SDK_12.3.0_d7731ad.zip  -d nRF5_SDK_12
    cd nRF5_SDK_12
```
  * Install git, OpenOCD and the gcc-arm compiler by going to the terminal and executing:
```
    sudo apt-get update # make packages upto date
    sudo apt install git
    git --version # display goit version
    sudo apt install openocd gcc-arm-none-eabi
```
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
  * Clone reversebias' Mitosis git repository by going to the terminal and executing:
```
    cd ~/nRF5_SDK_12
    git clone https://github.com/reversebias/mitosis
```
  * Install udev rules (copy from get to the /etc directory):
```
    sudo cp mitosis/49-stlinkv2.rules /etc/udev/rules.d/
```
  * Plug in, or replug in the programmer. (should light up!)
-------------------------------------------
The programming header on the side of the keyboard, from top to bottom:

    SWCLK
    SWDIO
    GND
    3.3V

It's best to remove the battery during long sessions of debugging, as charging non-rechargeable lithium batteries isn't recommended.

Launch a debugging session with:
```
    cd ~/nRF5\_SDK\_12
    openocd -f mitosis/nrf-stlinkv2.cfg
```
Should give you an output ending in:
```
    Info : nrf51.cpu: hardware has 4 breakpoints, 2 watchpoints
```

Otherwise you likely have a loose or wrong wire.
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


## update Ubuntu software.

(add here)

-------------------------------------------

## Install the extension pack with the same version as your installed version of VirtualBox!
Oracle VM VirtualBox Extension Pack
From: https://www.youtube.com/watch?v=mwKmxxRbvws

    * https://www.virtualbox.org/wiki/Downloads
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
-------------------------------------------


-------------------------------------------
Guest additions
When running the VM, select Devices->Insert Guest Additions CD Image. This popped up a prompt in my VM to install them

Copy/Paste is not by default. If you have installed Guest Additions, then you can do this:
    Start the VM
    Go to Machine > Settings in the file menu.
    Go to the General tab, then Advanced.
    Set the Shared Clipboard setting to Disabled, Guest to Host, Host to Guest or Bidirectional.

-------------------------------------------
