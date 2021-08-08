# Pi-Hole Configuration

The following guide walks through the steps to set up Pi-Hole on a fresh Raspbian installation. Steps are also included for the configuration of Jackett, Sonarr and Radarr.

## Raspbian


<img src="https://www.raspberrypi.org/app/uploads/2018/03/RPi-Logo-Reg-SCREEN.png" alt="" width="100 px">


The following steps are an abreviated version of the instructions for writing a Rasbpian image on a Linxu machine. The full set of instructions can be found [here](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md).


- Download the latest version of Raspbian:  
`wget https://downloads.raspberrypi.org/raspbian_lite_latest`

- Discover the SD card mount:  
`lsblk -p`

- Unmount any  SD card partitions that have been mounted. E.g. if the `sda1` partition is mounted, it can be unmounted with the following command:    
`umount /dev/sda1`

- Copy the Raspian image file to the SD card by running the following code:    
`unzip -p *-raspios-buster-armhf.zip | sudo dd of=/dev/sda bs=4M conv=fsyn`    
The code works as follows:    
 - `unzip` command will unzip the image file
 - `|` operator will pipe the output
 - `dd` command is used to convert and copy files
 - It is assumed that the SD card is mounted at `/dev/sda`
 - `conv=fsync` command will make  the device flush its buffers and caches so that if the device is removed the data is written to it before the operation is marked as complete and control passed back to the terminal prompt.




## Pi-Hole

### Overview

[Pi-Hole](https://pi-hole.net/) is 

Pi-Hole can also be configured to act as a DHCP server to 



Discovering the SD card mountpoint and unmounting it
Run lsblk -p to see which devices are currently connected to your machine.

If your computer has a slot for SD cards, insert the card. If not, insert the card into an SD card reader, then connect the reader to your computer.

Run lsblk -p again. The new device that has appeared is your SD card (you can also usually tell from the listed device size). The naming of the device will follow the format described in the next paragraph.

The left column of the results from the lsblk -p command gives the device name of your SD card and the names of any partitions on it (usually only one, but there may be several if the card was previously used). It will be listed as something like /dev/mmcblk0 or /dev/sdX (with partition names /dev/mmcblk0p1 or /dev/sdX1 respectively), where X is a lower-case letter indicating the device (eg. /dev/sdb1). The right column shows where the partitions have been mounted (if they haven't been, it will be blank).

If any partitions on the SD card have been mounted, unmount them all with umount, for example umount /dev/sdX1 (replace sdX1 with your SD card's device name, and change the number for any other partitions).

### Installation

Installation of Pi-Hole can be completed by running the following code in the Terminal :

```
git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
cd "Pi-hole/automated install/"
sudo bash basic-install.sh
```


## Jackett


## Sonarr


## Radarr





