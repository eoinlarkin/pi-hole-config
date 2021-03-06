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

<img src="https://wp-cdn.pi-hole.net/wp-content/uploads/2016/12/Vortex-R.png" alt="" width="100 px">

### Overview

[Pi-Hole](https://pi-hole.net/) is a Linux network-level advertisement and Internet tracker blocking application which acts as a DNS sinkhole and optionally a DHCP server, intended for use on a private network.

### Installation

Installation of Pi-Hole can be completed by completing the following steps:

- SSH into the Raspberry Pi. It is recommended to set a static IP address for the Pi; this can usually be done by access the router settings

- Update the packages on the Raspberry Pi:    
`sudo apt-get update && sudo apt-get upgrade`

- Install git on the Raspberry Pi:    
`sudo apt install git`

- Run the following code in Terminal :
    ```
    git clone --depth 1 https://github.com/pi-hole/pi-hole.git Pi-hole
    cd "Pi-hole/automated install/"
    sudo bash basic-install.sh
    ```

- Once Pi-Hole is installed, the admin password can be reset using the following command:   
`sudo pihole -a -p`

## Jackett

The following steps are taken from the official Jackett GitHub which can be found [here](https://github.com/Jackett/Jackett/blob/master/README.md).


- Install the Mono repository:    
```
    sudo apt install apt-transport-https dirmngr gnupg ca-certificates
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
    echo "deb https://download.mono-project.com/repo/debian stable-raspbianbuster main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list
    sudo apt update
```

- Install Mono:    
`sudo apt install mono-devel`

- Install libcurl:    
`apt-get install libcurl4-openssl-dev`

- Download and extract the latest `Jackett.Binaries.Mono.tar.gz` release from the [releases page](https://github.com/Jackett/Jackett/releases):    
`wget https://github.com/Jackett/Jackett/releases/download/v0.18.531/Jackett.Binaries.Mono.tar.gz`    
`tar -xvf *.tar.gz`    
`sudo mkdir /opt/jackett`    
`sudo mv Jackett/* /opt/jackett`    
`rm -rf Jackett`    
`sudo chown pi:pi /opt/jackett`    

- Jackett can be started by running:    
`mono --debug /opt/jackett/JackettConsole.exe`

- To install Jackett as a service, run `sudo /opt/jackett/install_service_systemd_mono.sh` To stop the service run `systemctl stop jackett.service` from Terminal. You can start it again it using `systemctl start jackett.service`. Logs are stored as usual under `~/.config/Jackett/log.txt` and also in `journalctl -u jackett.service`.

- Once Jackett is started, it can be access at the following address:    
`http://ip.address:9117` 


## Sonarr

IThe following steps are taken from the official Sonar website and can be found [here](https://sonarr.tv/#downloads-v3-linux-debian).

- Add the Sonarr repository:    
    ```
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 2009837CBFFD68F45BC180471F4F90DE2A9B4BF8
    echo "deb https://apt.sonarr.tv/debian buster main" | sudo tee /etc/apt/sources.list.d/sonarr.list
    sudo apt update
    ```

- Install Sonarr:    
`sudo apt install sonarr`

- Once install Sonarr can be accessed at the following address:

## Radarr

The following steps are taken from the official Radarr wiki which can be found [here](https://wiki.servarr.com/radarr/installation).


- Install the prequisites:    
`sudo apt install curl mediainfo sqlite3`

- Check the system architecture - this should return **armhf**:    
`dpkg --print-architecture`

- Download Radarr:    
`wget --content-disposition 'http://radarr.servarr.com/v1/update/master/updatefile?os=linux&runtime=netcore&arch=arm'`

- Unzip the archive:    
`tar -xvzf Radarr*.linux*.tar.gz`

- Move the Radarr directory to **opt**:    
`sudo mv Radarr/ /opt`

- Change ownership of the directory:
`sudo chown pi:pi /opt/Radarr`

- Raspbian has a version of *libseccomp2* that is too old to support Radarr. The recommended solution from teh Radarr FAQ is to install the backport of *libseccomp2* from the debian repository:
  - Check that you are running buster: `lsb_release -a`
  - Add the package key: `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 04EE7237B7D453EC 648ACFD622F3D138`
  - Add the backport to your sources `echo 'deb http://httpredir.debian.org/debian buster-backports main contrib non-free' | sudo tee -a /etc/apt/sources.list.d/debian-backports.list`
  - Install the backport of libseccomp2: `sudo apt update && sudo apt install libseccomp2 -t buster-backports`

- If there are issues with libseccomp2, it can be restored as follows:    
`wget http://raspbian.raspberrypi.org/raspbian/pool/main/libs/libseccomp/libseccomp2_2.5.1-1+rpi1_armhf.deb`    
`dpkg -i libseccomp2_2.5.1-1+rpi1_armhf.deb`

- Configure systemd so radarr can autostart at boot.
    ```
    cat << EOF | sudo tee /etc/systemd/system/radarr.service > /dev/null
    [Unit]
    Description=Radarr Daemon
    After=syslog.target network.target
    [Service]
    User=radarr
    Group=media
    Type=simple

    ExecStart=/opt/Radarr/Radarr -nobrowser -data=/data/.config/Radarr/
    TimeoutStopSec=20
    KillMode=process
    Restart=always
    [Install]
    WantedBy=multi-user.target
    EOF
    ```

- Reload systemd    
`sudo systemctl -q daemon-reload`

- Enable the Radarr service:    
`sudo systemctl enable --now -q radarr`
