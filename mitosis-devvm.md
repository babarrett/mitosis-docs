# Creating a VM for Mitosis Development

Overview:
* Install VirtualBox
* Create a VM
    * 2GB RAM
    * 30GB HD, dynamic
    * ...
* Install Ubuntu v16.x
    * Add Oracle guest-additions
    * 
Install ST-LINK V2 programer


Install OpenOCD programming software


Git Mitosis repository
    * Make sure length is OK
    





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


---------------------guest-additions----------------------
(Don't need?)
get guest additions...
ssh into guest OS (or run terminal)
follow these instructions

https://superuser.com/questions/527507/use-ubuntu-server-as-web-server-on-mac-os-x-via-virtualbox/527508#527508

and you can add shared folders to OSX here too.

```
sudo apt-get update
sudo apt-get install dkms
sudo apt-get install virtualbox-guest-additions-iso
sudo apt-get install build-essential linux-headers-$(uname -r)
sudo apt-get install virtualbox-ose-guest-x11
```
Restart Ubuntu
-------------------------------------------
-------------------------------------------
Connect ST-LINK V2 to 
