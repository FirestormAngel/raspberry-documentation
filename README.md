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
* Chapter 0x06: Installing and configuring components for Wifi 802.11ac with WPA2-PSK
* Chapter 0x07: Installing and configuring components for DNS, DHCP, iptables
* Chapter 0x08: Installing and configuring components for IPSec, iptables
* Chapter 0x09: Configuring scheduled crontab NMAP scans of your wifi network
* Chapter 0x0A: Configuring and securing the SSH server access

### This will probably work on the 4GB and 8GB Raspberry Pi 4 models. (later release date)
* Chapter 0x0B: Hardware and Requirements
* Chapter 0x0C: Installing and configuring components for Docker (repo and installation)
* Chapter 0x0D: Installing and configuring components for Python3 environments.
* Chapter 0x0E: Installing and configuring pcap recorder.
* More chapters in the pipe.

### Set the right expectations

NOTE:
Most of these components already exist and comes via the very well version managed Debian Buster repository. It should be somewhat safe to use, however I have had experiences of opensource software suddenly stopped working due to version handling and kernel differences. This may happen, and in such cases it might take time to get the problems fixed. If possible then dont update the firmware versions on your raspberry on your own, let them come with the repository updates.

1) **Myth** People have a tendency to misunderstand the use-case of wifi accesspoint signal range. The raspberry cannot produce long range wifi, and the businesscase is actually to stay within your house, and securely access the wifi. If you want a long range, you can buy a an external USB wifi adapter and use that as an accesspoint card. However, all wificards are not capable of being used as a Accesspoint. In some cases, your might even have to re-compile the sourcecode from the hostapd. I will, in my efforts try to produce or point to a compatible list of wifi adapters.

2) **Myth** People have a tendency to misunderstand the use-case of data throughput. Yes, you will ofcourse be able to watch 2 different netflix movies on two different mobilephones or update your mobilephones with updates. The Raspberry will make it possible, but there are ofcourse limitations. For example, I have made this work with 20/20Mhz band, and unfortunately been unsuccessful trying 40/40Mhz bands with the built-in wificard.

*Read carefully:*
I also publish this information because I care about your safety, I expect you to use this extensive knowledge **wisely** and **not** maliciously. If you find problems or detect security issues, don't hesitate to inform me. I expect you to do that.

*Read carefully:*
I strongly urge you to **not** put this device directly on the Internet, unless you know exactly what you are doing. I suggest atleast put it behind a NAT router, a firewall or atleast some kind of transit network.

# Chapters

### Chapter 0x00: Hardware and Requirements
in progress, and subject to change

* 1 Raspberry Pi 4, 4GB RAM or higher.
* 1 Raspberry Pi 4, power adapter with USB-C, 5V/3A.
* 1 GeekWorm Raspberry Pi 4 Armor heatsink.
* 2 SD Cards, 32GB.
* 1 USB 3.0 SD card reader.
* 1 Network Cable.
* 1 Network cable port, available on your switch, router, firewall or transit network.

Warning! This device will be warm. This device is going to be online most of the day, and carrying a heavy load, so a proper aluminium armor without fan is probably a good idea. Just make sure the heated components are properly attached and leading away the heat from the components. I have had success with the GeekWorm Raspberry Pi 4 Armor Gray, thus leading the cpu at +53C.


### Chapter 0x01: Installing the raspberry SD card
in progress, and the instruction is subject to change

**Instruction here on installing image from a Windows system**

in progress, and the instruction is subject to change

* [Instruction here].


**Instruction here on installing image from a Raspberry system**

I'm currently working on a Raspberry Pi with a Desktop, this is why the SD card instruction looks like this at the moment. Instructions for Windows will come in a while. But for now, I'm on a raspberry desktop.

I have based my Raspberry Pi wifi router image on the Raspbian Buster Lite version, but I'm pretty sure Raspberry Pi OS Buster version will work the same way as the previous versions. This will be tested extensively before the official release date 2020-07-01. At the time of this writing, this OS has not been tested.

* [Instruction here].

Download the **Raspberry Pi OS (32-bit) Lite** from **The Rasberry Pi foundation**, [here](https://www.raspberrypi.org/downloads/raspberry-pi-os/).

Open a console and prepare your image file to be written to the SD card.
```bash
$ sudo apt-get update
$ sudo apt-get install dcfldd gunzip
```
Note: you can use the **dd** command, I just like the progress indicator of **dcfldd**.

Go to the Downloads folder
```bash
$ cd Downloads
$ ls -l
total 16469060
...
-rw-r--r-- 1 pi pi  452715448 jun 14 10:08  2020-05-27-raspios-buster-lite-armhf.zip
...
```

Verify the sha256 hash, like so, and verify the red text output in the console.
```bash
$ sha256sum 2020-05-27-raspios-buster-lite-armhf.zip | grep f5786604be4b41e292c5b3c711e2efa64b25a5b51869ea8313d58da0b46afc64
...
f5786604be4b41e292c5b3c711e2efa64b25a5b51869ea8313d58da0b46afc64  2020-05-27-raspios-buster-lite-armhf.zip
...
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
[18380.662439] sd 0:0:0:1: [sdb] 62333952 512-byte logical blocks: (31.9 GB/29.7 GiB)
[18380.672670]  sdb: sdb1 sdb2
...
```
The last 2 lines contain a SD card and its partitions detected, in this case **sdb** (disk), **sdb1** (partition1) and **sdb2**  (partition2), because this is a raspberry card. Your output might be different, it might be **sda** or **sdb**.

To overwrite the SD card **sdb** with your new raspberry image. Please do note the warning, **This will indeed delete all previous contents of your SD card**, make sure its the correct card in your reader.
```bash
$ sudo dcfldd if=2020-05-27-raspios-buster-lite-armhf.img of=/dev/sdb bs=4M
```

* [Instruction here].



### Chapter 0x02: Update the Debian Buster image file
in progress

* Instruction on making a preconfigured Golden-Image here.


### Chapter 0x03: Configuring IP addresses on eth0 and wlan0
in prgress, also subject to change

In this section we will assign static IPv4 addresses on your raspbian. If you have other networks you should assign them to the configuration below, and use the correct network and subnetmask for your network. I will however be consistent here, most people with a little knowledge do understand that if you work with this system remotely, you will be disconnected and will need to reconnect to the device, while restarting the service.

* **Advice**: If you work with this device remotely, make sure you are entering the correct information, and that you are able to connect to it afterwards. Changeing the IP address may render the device unavailable, even the device is online.

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
    # no routers
    # no domain_name_servers
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
Thankyou for reading this section, this will help you in our later sections during logging, evidence and negotiating ipsec and prevent you from getting errors later when we use transport layer security with certificates. They will depend on you completing this section.


### Chapter 0x05: Installing and configuring components for RSyslog
in progress


### Chapter 0x06: Installing and configuring components for Wifi 802.11ac with WPA2-PSK
in progress, also subject to change

In this chapter we are going to install the hostapd, an essential component for making your Raspberry Pi 4 a wifi access point. 

* **Advice** This step will only enable the hostapd and make it start broadcasting its ssid name.
* **Advice** Chapter 0x07, 0x08 are required to make the access point work.
* **Advice** You will need to disable the wpa_supplicant.service

If you are using your Raspberry Pi 4, wifi to connect to another accesspoint, now is the time to stop doing that.

Update and upgrade your OS, then install the Wifi AccessPoint daemon.
```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install hostapd -y
```

Make sure the hostapd.service is stopped.
```bash
$ sudo systemctl stop hostapd
```

Start by disabling the wpa_supplicant.service, which makes your raspberry a client to an external access point. From now on your raspberry is going to be the access point.
```bash
sudo systemclt stop wpa_supplicant.service
sudo systemctl disable wpa_supplicant.service

# and if you really want to be sure its not used, then..
sudo mv /etc/wpa_supplicant/wpa_supplicant.conf /etc/wpa_supplicant/wpa_supplicant.conf.old
```


Make sure your *hostapd.conf* is pointed out inside */etc/default/hostapd*.
```bash
$ sudo nano /etc/default/hostapd
```
```bash
    # Defaults for hostapd initscript
    #
    # WARNING: The DAEMON_CONF setting has been deprecated and will be removed
    #          in future package releases.
    #
    # See /usr/share/doc/hostapd/README.Debian for information about alternative
    # methods of managing hostapd.
    #
    # Uncomment and set DAEMON_CONF to the absolute path of a hostapd configuration
    # file and hostapd will be started during system boot. An example configuration 
    # file can be found at /usr/share/doc/hostapd/examples/hostapd.conf.gz
    #
    #DAEMON_CONF=""

    # Additional daemon options to be appended to hostapd command:-
    #       -d   show more debug messages (-dd for even more)
    #       -K   include key data in debug messages
    #       -t   include timestamps in some debug messages
    #
    # Note that -B (daemon mode) and -P (pidfile) options are automatically
    # configured by the init.d script and must not be added to DAEMON_OPTS.
    #
    #DAEMON_OPTS=""

    # yes, enter it here.. even its deprecated.
    DAEMON_CONF="/etc/hostapd/hostapd.conf"

```

I have made a couple of hostapd configuration examples for you to select from. 
* **Advice** Please select the one that is most appropriate for you.
* **Advice** Read up on your documentation for your devices before selecting the one to choose.

```bash
$ sudo nano /etc/hostapd/hostapd.conf
```

Example configuration 1: Wireless 802.11ac on 5Ghz, on channel 48, bandwidth 20/20Mhz. Recommended for iPhone 6 and upwards.
```bash

    ########################################
    ## This configuration will work with  ## 
    ## newer iPhone models, 6s and newer  ##
    ## Android phones.                    ##
    ##                                    ##
    ## However Smart TV's might not work  ##
    ## with this as some of them still    ##
    ## use 802.11b/g/n on the 2.4Ghz      ##
    ## band.                              ##
    ##                                    ##
    ## Check your documentation.          ##
    ##                                    ##
    ## This configuration is recommended  ##
    ## for devices that have VPN          ##
    ## capability.                        ##
    ##                                    ##
    ## Check your documentation.          ##
    ########################################
    
    # attach this hostapd to wlan0 (builtin wireless card)
    interface=wlan0
    
    # always use the nl80211 driver, unless otherwise specified.
    driver=nl80211
    
    # enable the 5Ghz band.
    hw_mode=a
    
    # Channel 36 or 48 is probably your best option for 20/20Mhz band.
    # Important, if you run multiple Raspberries as accesspoints next to eachother, I urge you to use different channels.
    channel=48
    
    # Country Wireless compliance code. (enter your country code: Example GB, US or SE)
    country_code=SE
    
    # Wireless compliance 802.11d (1=enable, 0=disable)
    ieee80211d=1
    
    # Wireless compliance 802.11n (1=enable, 0=disable)
    ieee80211n=1
    
    # Wireless compliance 802.11ac (1=enable, 0=disable)
    ieee80211ac=1
    
    # new variable, untested.
    #wme_enabled=1
    
    # bandwith controller.
    wmm_enabled=1

    # bandwith controller. 40/40Ghz example. Does not work.
    # ht_capab=[HT40+][SHORT-GI-80][DSSS_CCK-40]

    # macaddress acl (1=enabled, 0=disabled)
    macaddr_acl=0

    # hide ssid broadcasts (1=enabled, 0=disabled)
    ignore_broadcast_ssid=0
    
    ########################################
    ## Wifi settings here                 ##
    ########################################

    # enter ssid name here, make sure it does not conflict with another accesspoint.
    ssid=wifi-03.firestorm.org
    
    # enable authentication (1=enable, 0=disable)
    auth_algs=1
    
    # enforce wpa2 algorithms. 1=wpa1, 2=wpa2, 3=combinded wpa1 and wpa2 (not recommended)
    wpa=2
    
    # enforce use of pre-shared key.
    wpa_key_mgmt=WPA-PSK
    
    # enforce the CCMP encryption algorithm
    rsn_pairwise=CCMP
    
    # enforce hostapd with your preshared key.
    wpa_passphrase=<enter your password here. 20-32 characters recommended.>
    
```

Enable traffic forwarding from your wlan0 card to eth0 physical network card.
```bash
$ sudo nano /etc/sysctl.conf
```

Example: Find *net.ipv4.ip_forward* inside your *sysctl.conf* and set it to *1*.
```bash
    net.ipv4.ip_forward=1
```

The hostapd is masked, which means you cannot enable it per default. Unmask it and then enable it.
```bash
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
```

Example: Check status of the hostapd.service
```bash
$ sudo systemctl status hostapd.service
```

Example: Start the hostapd.service
```bash
$ sudo systemctl start hostapd.service
```

Example: Stop the hostapd.service
```bash
$ sudo systemctl stop hostapd.service
```


### Chapter 0x07: Installing and configuring components for DNS, DHCP, iptables
in progress, subject to change

In this chapter we are going to enable dhcp and dns which will enable your accesspoint to configure your wifi attached devices with the ip addresses they will be using to navigate the wifi network. I'll be using dnsmasq since this probably is the most qualified software for this task. Dnsmasq is widely used in routers and appliances for both dhcp and dns navigation. If you shoud select something, then select dnsmasq. In a few moments you'll understand why.

I'm repeating the installation of dnsutils, tcpdump, nmap, because you will need them later in this chapter.

update, upgrade and install dnsmasq
```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install dnsmasq dnsutils tcpdump nmap -y
```
```bash
$ sudo systemctl stop dnsmasq
$ sudo systemctl stop hostapd
```

Rename the dnsmasq.conf to old and start a new dnsmasq.conf file.
```bash
$ sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
```
```bash
$ sudo nano /etc/dnsmasq.conf
```

Add the following entries.
```bash
    local=/localnet/

    # exmple on howto re-route ntp dns requests to another network time protocol server.
    #address=/ntp.org/193.11.166.2
    #address=/time-ios.apple.com/193.11.166.2

    addn-hosts=/etc/dnsmasq/hostile_hosts

    expand-hosts
    domain=internal.firestorm.org
    domain=wifi.firestorm.org,192.168.230.0/24
    # we havent added docker yet, but this entry would be correct.
    #domain=docker.firestorm.org,172.17.0.0/16

    # listen for DNS request on these addresses.
    listen-address=192.168.230.254
    listen-address=192.168.220.30
    # we havent added docker yet, but this entry would be correct.
    #listen-address=172.17.0.1
    listen-address=127.0.0.1

    # no dhcp responses on eth0
    no-dhcp-interface=eth0
    # we havent added docker yet, but this entry would be correct.
    #no-dhcp-interface=docker0

    # dhcp wifi network
    dhcp-range=192.168.230.10,192.168.230.30,255.255.255.0,6h
    dhcp-option=option:ntp-server,193.11.166.2,193.11.166.18
    dhcp-option=option:dns-server,192.168.230.254
    dhcp-option=option:domain-search,wifi.firestorm.org
    # make wpad behave on wifi.
    dhcp-option=252,"\n"
    # authorative dhcp on wifi network.
    dhcp-authoritative
    
    # example, you can add your own command to run when a client requests a dhcp entry.
    dhcp-script=/bin/echo

    srv-host=_ipp._tcp,printer.internal.firestorm.org,631
    srv-host=_raw._tcp,printer.internal.firestorm.org,9100
    txt-record=_http._tcp.printer.internal.firestorm.org,name=value,paper=A4

    log-queries
    log-dhcp

    conf-dir=/etc/dnsmasq.d/,*.conf

```

```bash
$ sudo mkdir /etc/dnsmasq/
$ sudo nano /etc/dnsmasq/hostile_hosts
```

```bash
    ########################################
    ## HOSTILE                            ##
    ########################################

    # redirector examples
    # redirect url of choice to ip of choice
    127.0.0.1   *.hostile-web-server.com

```




### Chapter 0x08: Installing and configuring components for IPSec, iptables
in progress


### Chapter 0x09: Configuring scheduled crontab NMAP scans of your wifi network
in progress


### Chapter 0x0A: Configuring and securing the SSH server access
in progress
