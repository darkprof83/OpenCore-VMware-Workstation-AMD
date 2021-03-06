# OpenCore 0.6.5 - VMware Workstation 16 - AMD Ryzen
Install macOS Big Sur on Ryzen with VMware Workstation and OpenCore

![Alt text](https://github.com/Ken5998/OpenCore-VMware-Workstation-AMD/blob/main/images/macos.jpg?raw=true "VMware screenshot")

**The VM is succesfully installed and tested on 2 different cpus/systems:**
* **Ryzen 9 3900X** -> VM has 6 core CPU and 8GB RAM
* **Ryzen 5 PRO 4650U** -> 4 core CPU and 8GB RAM

## Prerequisites
* Real Mac or a VM with macOS
* USB 16GB for the #Method 2
* Pre-build VMDK (macOS Big Sur 11.1) for the #Method 1
* VMware Workstation 16 (Player and Workstation 15 should also work) 
* Unlocker for VMware -> [unlocker](https://github.com/BDisp/unlocker)


## Method 1
### Download the pre-build VMDK for the installer of macOS Big Sur 11.1 from here: 
[VMDK file](https://drive.google.com/file/d/10qLPTret3KoV1bMRrcHNqKoN7mHvn2-6/view?usp=sharing)

[Alternative link](https://1fichier.com/?latap9wd4snffk0h4yon)

- Close VMware Workstation
- Download the Unlocker and run win-install.cmd as Admin
- Open VMware Workstation and create a new VM
- I will install the operating system later
- Select **Apple Mac OS X** -> macOS 11.1
- Choose a name for the Virtual Machine
- I created a disk size of 200GB (reccomended is 80GB)
- Finish 
- After that modify the VM and attach the downloaded VMDK

- Now start the VM, install macOS and continue with the [Post-installation](https://github.com/Ken5998/OpenCore-VMware-Workstation-AMD#post-installation) :)

## Method 2
### First of all you need to prepare the USB that we will use to install macOS on VMware
### Instructions
- Download macOS Big Sur from the App Store.
- Plug in an empty USB drive.
- Run one of the below commands in your Terminal to prepare the bootable macOS USB.
```
NOTE: Make sure to replace 'MyVolumeName' with your actual USB volume name in the below commands.

## Big Sur
sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolumeName

## Big Sur Beta
sudo /Applications/Install\ macOS\ Big\ Sur\ Beta.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolumeName
```

### EFI partition
You can use this tool: [MountEFI](https://github.com/corpnewt/MountEFI)
* Just copy the EFI folder into the EFI partition that you have mounted

### Pre-installion (Windows)
Close VMware Workstation

Download the Unlocker and run win-install.cmd as Admin

Open VMware Workstation and create a new VM
* I will install the operating system later
* Select **Apple Mac OS X** -> macOS 11.0 or macOS 11.1
* Choose a name for the Virtual Machine
* I created a disk size of 200GB (reccomended is 80GB)
* Finish

### macOS Installation
* Start the VM and select **No**
* Insert the USB into the PC and connect it to the VM
* On the VM "bios" select **Reset the system**
* After the reboot you will see that the VM is booting into the OpenCore menu
* Select **Install macOS Big Sur (external)** | or Beta if you are installing macOS 11.1

After that is a "normal" installation of a macOS with OpenCore, you need to create a new partition and install macOS

NOTE: The installation require a lot of reboot so don't panic :)

## Post-installation
After the complete installation of macOS there are some changes to do:

**EFI FOLDER**
* Download the EFI folder from the [releases](https://github.com/Ken5998/OpenCore-VMware-Workstation-AMD/releases) page
* Copy the folder of the USB into the EFI partition of the VM
* Download the [MountEFI](https://github.com/corpnewt/MountEFI) software and mount the partition of the USB and the macOS Disk
* Replace the content of the macOS disk with the USB EFI content

After that **eject** the USB and shutdown the VM.

**OpenCore entry into the VM BIOS**

Select the VM and near the Power button select **Power On to Firmware** from the dropdown list
* Select **Enter setup**
* Configure boot options
* Delete boot option -> remove OpenCore (this is the OpenCore from the USB or VMDK installer)
* Add boot option -> search for **EFI, [PciRoot...** and select it
* Select **<EFI>** -> **<BOOT>** -> **BOOTx64.efi**
* Input the description -> insert something like OpenCore AMD or just Opencore
* Commit changes and exit
* Select **Configure boot options**
* Change boot order -> set the OpenCore entry at first position, then **Commit changes and exit**
* Exit the Boot Maintenance Manager
* Select **OpenCore** and boot to macOS

**VMware Tools**

Now if you want you can install the VMware Tools to resize correctly the VM and to enable the drag&drop from the VM to the host and viceversa
* The VMware tools are inside the Unlocker folder under tools -> darwin.iso

**Generate new SMBIOS info with OpenCore Configurator or something else :)**

## FIX
**AUDIO**
* If exists AppleALC.kext in your EFI folder delete it.
* Download and install VoodooHDA OC from: [VoodooHDA OC](https://github.com/chris1111/VoodooHDA-OC)
* Reboot the system
* Select **SPDIF-out** from the audio outputs


## Credits
- [AlGrey](https://github.com/AlGreyy) for creating the patches.
- [XLNC](https://github.com/XLNCs) for maintaining patches to various macOS versions.
- Sinetek, Andy Vandijck, spakk, Bronya, Tora Chi Yo, Shaneee and many others for sharing their AMD/XNU kernel knowledge