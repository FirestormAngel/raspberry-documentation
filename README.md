# Raspberry-Documentation (release date 2020-07-01)
Official release date for the most basic installation instructions will be official here on 2020-07-01.
Scripts and automation will come at a later date, when its more mature.

Raspberry Pi 4 is a very capable little creditcard sized computer with a 4 Core ARM CPU. It comes with different boards, sizing 1GB, 2GB, 4GB and 8GB RAM. I have, for the most basic things in my test and online systems, always chosen the 4GB model and a 32GB SD card. Do not underestimate, these applications require both memory and CPU. Running it on Raspberry Pi 2, 3, 3B+, yes, it can be installed but will most likely fail or not perform well. Trust me, I've tried it already, I've been working on this for years.

It should also be said, most of these chapters will come with extensive explanations, why and why not to do things. You might be able to follow if you are an intermediate to advanced user familiar with linux bash and configuration.

### This will probably work on all Raspberry Pi 4 models.
* Chapter 0x00: Hardware and Requirements
* Chapter 0x01: Installing the raspberry SD card
* Chapter 0x02: Update the Debian Buster image file
* Chapter 0x03: Configuring IP addresses on eth0 and wlan0
* Chapter 0x04: Synchronizing the time with Network Time Servers
* Chapter 0x05: Installing and configuring components for RSyslog
* Chapter 0x06: Installing and configuring components for Wifi 802.11ac
* Chapter 0x07: Installing and configuring components for DNS, DHCP, iptables
* Chapter 0x08: Installing and configuring components for IPSec, iptables
* Chapter 0x09: Configuring scheduled crontab NMAP scans of your wifi network.

### This will probably work on the 4GB and 8GB Raspberry Pi 4 models. (later release date)
* Chapter 0x0A: Hardware and Requirements
* Chapter 0x0B: Installing and configuring components for Docker (repo and installation)
* Chapter 0x0C: Installing and configuring components for Python3 environments.
* Chapter 0x0D: Installing and configuring WASP pcap recorder.
* More chapters in the pipe.

NOTE:
Most of these components already exist and comes via the very well version managed Debian Buster repository. It should be somewhat safe to use, however I have had experiences of opensource software suddenly stopped working due to version handling and kernel differences. This may happen, and in such cases it might take time to get the problems fixed. If possible then dont update the firmware versions on your raspberry on your own, let them come with the repository updates.

*Read carefully:*
I also publish this information because I care about your safety, I expect you to use this extensive knowledge **wisely** and **not** maliciously. If you find problems or detect security issues, don't hesitate to inform me. I expect you to do that.

*Read carefully:*
I strongly urge you to **not** put this device directly on the Internet, unless you know exactly what you are doing. I suggest atleast put it behind a NAT router, a firewall or atleast some kind of transit network.

# Chapters

### Chapter 0x00: Hardware and Requirements
in progress, and subject to change

* 1 Raspberry Pi 4, 4GB RAM or higher.
* 2 SD Cards, 32GB.
* 1 USB 3.0 SD card reader.
* 1 Network Cable.
* 1 Network cable port, available on your switch, router, firewall or transit network.

You need to find out what network you are on.


### Chapter 0x01: Installing the raspberry SD card
in progress, and subject to change

I have based my Raspberry Pi wifi router image on the Raspbian Buster Lite version, but I'm pretty sure Raspberry Pi OS Buster version will work the same way as the previous versions. This will be tested extensively before the official release date 2020-07-01. At the time of this writing, this OS has not been tested.

Download the **Raspberry Pi OS (32-bit) Lite** from **The Rasberry Pi foundation**, [here](https://www.raspberrypi.org/downloads/raspberry-pi-os/).

Verify the sha256 hash, like so, and verify the red text output in the console.
```bash
$ sha256sum 2020-05-27-raspios-buster-lite-armhf.zip | grep f5786604be4b41e292c5b3c711e2efa64b25a5b51869ea8313d58da0b46afc64
```
Extract the image with the gunzip command.
```bash
$ gunzip 2020-05-27-raspios-buster-lite-armhf.zip
```

Insert your USB 3.0 SD card reader, and run the following command.
```bash
$ dmesg
```
```bash
...
[18217.822017] usb 2-1: new SuperSpeed Gen 1 USB device number 2 using xhci_hcd
[18217.881582] usb 2-1: New USB device found, idVendor=0bda, idProduct=0326, bcdDevice=11.24
[18217.881598] usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[18217.881611] usb 2-1: Product: USB3.0 Card Reader
[18217.881623] usb 2-1: Manufacturer: Realtek
[18217.881635] usb 2-1: SerialNumber: 201404081410
[18217.899942] usb-storage 2-1:1.0: USB Mass Storage device detected
[18217.901303] scsi host0: usb-storage 2-1:1.0
[18218.991413] scsi 0:0:0:0: Direct-Access     Generic- USB3.0 CRW   -SD 1.00 PQ: 0 ANSI: 6
[18219.003925] scsi 0:0:0:1: Direct-Access     Generic- USB3.0 CRW   -SD 1.00 PQ: 0 ANSI: 6
[18219.006767] sd 0:0:0:0: [sda] Attached SCSI removable disk
[18219.014432] sd 0:0:0:1: [sdb] Attached SCSI removable disk
[18219.022467] sd 0:0:0:0: Attached scsi generic sg0 type 0
[18219.022666] sd 0:0:0:1: Attached scsi generic sg1 type 0
...

```




To write the image to your empty SD card (Warning! will delete the contents of your card, make sure its the correct one)
```bash
$ dcfldd if=2020-05-27-raspios-buster-lite-armhf.img of=/dev/sda bs=4M
```



### Chapter 0x02: Update the Debian Buster image file
in progress



### Chapter 0x03: Configuring IP addresses on eth0 and wlan0
in prgress, also subject to change

In this section we will assign static IPv4 addresses on your raspbian. If you have other networks you should assign them to the configuration below, and use the correct network and subnetmask for your network. I will however be consistent here, most people with a little knowledge do understand that if you work with this system remotely, you will be disconnected and will need to reconnect to the device, while restarting the service.

* *Advice*: If you work with this device remotely, make sure you are entering the correct information, and that you are able to connect to it afterwards. Changeing the IP address may render the device unavailable, even the device is online.

```bash
$ sudo nano /etc/dhcpcd.conf
```
And add/modify the the eth0 static section:
```bash
    interface eth0
    static ip_address=192.168.220.30/24
    static routers=192.168.220.254
    static domain_name_servers=1.1.1.1 8.8.8.8
```
And add/modify the wlan0 static section:
```bash
    interface wlan0
    static ip_address=192.168.230.254/24
```
Note: I will add IPv6 configuration later, and when I do, it will work perfectly with the raspbian networking.




### Chapter 0x04: Synchronizing the time with Network Time Servers
in progress, also subject to change

Since your raspberry doesnt have a physical clock, its important to have it synchronize its clock with the configured network time servers available. Like anyone working with Cyber security, correct timestamps is of the essence in any evidence or logs. This to determine the correct timestamp something happened. Failing to do so, will render logs and evidence unusable.

Ok, you don't really have to change the *timesynccd* service, because it comes preinstalled on Debian Buster, however it is a good idea to have knowledge about this service existance, and that you don't need to install the ntp service. The NTP service which in turn will open port UDP/123, opening a footprint for traffic towards your Raspberry. The ntp service is complicated to secure, so unless you know how, don't use it.

```bash
$ sudo nano /etc/systemd/timesyncd.conf
```
Example: I've made an example if you want to choose your own NTP servers.
```configuration
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See timesyncd.conf(5) for details.

[Time]
NTP=ntp3.sptime.se ntp4.sptime.se
#FallbackNTP=0.debian.pool.ntp.org 1.debian.pool.ntp.org 2.debian.pool.ntp.org 3.debian.pool.ntp.org
#RootDistanceMaxSec=5
PollIntervalMinSec=64
PollIntervalMaxSec=2048
```
* **Advice**: You can uncomment the *FallbackNTP* and *RootDistanceMaxSec* if you want to have a NTP fallback and make sure your NTP servers answer within 5 seconds. This is recommended.

* **Advice**: The *PollIntervalMinSec* and *PollIntervalMaxSec* is the interval frame between sending ntp requests to the destinations. A value below 64 seconds is *not* recommended, unless you wish to be *blacklisted* on the ntp providers. *So, don't go below 64 seconds*.


Example: Check if the time is synchronized.
```bash
$ timedatectl status
```
```bash

               Local time: sön 2020-06-14 10:44:44 UTC
           Universal time: sön 2020-06-14 10:44:44 UTC
                 RTC time: n/a
                Time zone: Etc/UTC (UTC, +0000)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

Example: Turning NTP on or off. Please do turn it on with *true*.
```bash
$ sudo timedatectl set-ntp false
$ sudo timedatectl set-ntp true
```

Example: List available timezones. Grep if you want a shorter list.
```bash
$ timedatectl list-timezones
$ timedatectl list-timezones | grep America
$ timedatectl list-timezones | grep Europe
$ timedatectl list-timezones | grep Australia/Sydney
```

Example: Set timezone.
```bash
$ sudo timedatectl set-timezone Europe/Paris
$ sudo timedatectl set-timezone Europe/Stockholm
$ sudo timedatectl set-timezone Australia/Sydney
```
Example: Check the status the **systemd-timesyncd** service.
```bash
$ systemctl status systemd-timesyncd
```
Example: Restart the **systemd-timesyncd** service.
```bash
$ systemctl restart systemd-timesyncd
```
* Thankyou for reading this section, this will help you in our later sections during logging, evidence and negotiating ipsec and prevent you from getting errors later when we use transport layer security with certificates. They will depend on you completing this section.


### Chapter 0x05: Installing and configuring components for RSyslog
in progress


### Chapter 0x06: Installing and configuring components for Wifi 802.11ac
in progress


### Chapter 0x07: Installing and configuring components for DNS, DHCP, iptables
in progress


### Chapter 0x08: Installing and configuring components for IPSec, iptables
in progress

