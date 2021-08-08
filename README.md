# Pi-Hole Configuration

The following guide walks through the steps to set up Pi-Hole on a fresh Raspbian installation. Steps are also included for the configuration of Jackett, Sonarr and Radarr.

## Raspbian


<img src="https://www.raspberrypi.org/app/uploads/2018/03/RPi-Logo-Reg-SCREEN.png" alt="" width="100 px">


The following steps are an abreviated version of the instructions for writing a Rasbpian image on a Linxu machine. The full set of instructions can be found [here](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md).


- Download the latest version of Raspbian. The link to the latest version can be found on the following webpage: [link](https://www.raspberrypi.org/software/operating-systems/)  
`wget https://downloads.raspberrypi.org/raspios_lite_armhf/images/raspios_lite_armhf-2021-05-28/2021-05-07-raspios-buster-armhf-lite.zip`

- Discover the SD card mount:  
`lsblk -p`

- Unmount any  SD card partitions that have been mounted. E.g. if the `sda1` partition is mounted, it can be unmounted with the following command:    
`sudo umount /dev/sda1`

- Copy the Raspian image file to the SD card by running the following code:    
`unzip -p *-raspios-buster-armhf-lite.zip | sudo dd of=/dev/sda bs=4M conv=fsync status=progress`
  - `unzip` command will unzip the image file
  - `|` operator will pipe the output
  - `dd` command is used to convert and copy files
  - It is assumed that the SD card is mounted at `/dev/sda`
  - `conv=fsync` command will make  the device flush its buffers and caches so that if the device is removed the data is written to it before the operation is marked as complete and control passed back to the terminal prompt.


- After the .img file has been written, check that the SD card is unmounted and remove and reconnect the card.

- Create an empty `ssh` file - this will be loaded when Raspbian is first booted and will allow us to ssh into the Raspberry Pi:    
`touch /media/USER/boot/ssh`

- Write the WiFi credentials to the Raspberry Pi as follows:    
    ```
    echo "ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    country=IE
    network={
            scan_ssid=1
            ssid="<Name of your wireless LAN>"
            psk="<Password for your wireless LAN>"
            proto=RSN
            key_mgmt=WPA-PSK
            pairwise=CCMP
            auth_alg=OPEN
    }" > /media/USER/boot/wpa_supplicant.conf
    ```
- Unmount each SD Card partition - there should be two:    
`sudo umount /dev/sda1`    
`sudo umount /dev/sda2`

- Once the Raspberry Pi has been connected to the LAN, ssh into the Pi in order to setup the various applications. To ssh, use the following command:    
`ssh pi@<ip address>`    
The IP addresses of all devices on the current network can be found as follows:
`sudo nmap -sn <Router IP address>/24`



## Pi-Hole

### Overview

[Pi-Hole](https://pi-hole.net/) is a Linux network-level advertisement and Internet tracker blocking application which acts as a DNS sinkhole and optionally a DHCP server, intended for use on a private network.

### Installation

Installation of Pi-Hole can be completed by completing the following steps:

- SSH into the Raspberry Pi. It is recommended to set a static IP address for the Pi; this can usually be done by access the router settings

- Update the packages on the Raspberry Pi:    
`sudo apt-get update && sudo apt-get upgrade`

- Install git on the Raspberry Pi:


- Run the following code in Terminal :
    ```
    git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
    cd "Pi-hole/automated install/"
    sudo bash basic-install.sh
    ```


## Jackett


## Sonarr


## Radarr





