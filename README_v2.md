# Raspberry-Documentation
Official release date for the most basic installation instructions will be official here.
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
* Chapter 0x0E: Installing and configuring pcap recorder
* Chapter 0x0F: Configuration of Python environment
* Chapter 0x10: Configuration of Python environment for plugins for wifi-triangulation
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
* 1 USB 3.0 Memorystick, Kingston 64GB or higher. (used for logs and pcap)

Warning! This device will be warm. This device is going to be online most of the day, and carrying a heavy load, so a proper aluminium armor without fan is probably a good idea. Just make sure the heated components are properly attached and leading away the heat from the components. I have had success with the GeekWorm Raspberry Pi 4 Armor Gray, thus leading the cpu at +53C.

### Chapter 0x01: Installing the raspberry ubuntu SD card
in progress, and subject to change

* Installing image from a Windows system.
* Installing image from a Raspberry system.

### Chapter 0x02: Update the Debian Buster image file
in progress, and subject to change

### Chapter 0x03: Configuring IP addresses on eth0 and wlan0
in progress, and subject to change

In this section we will assign static IPv4 addresses on your raspbian. If you have other networks you should assign them to the configuration below, and use the correct network and subnetmask for your network. I will however be consistent here, most people with a little knowledge do understand that if you work with this system remotely, you will be disconnected and will need to reconnect to the device, while restarting the service.

* **Advice**: If you work with this device remotely, make sure you are entering the correct information, and that you are able to connect to it afterwards. Changeing the IP address may render the device unavailable, even the device is online.

```bash
$ sudo nano /etc/netplan/10-router.conf
```

And add/modify the the eth0 static IPv4 and IPv6 sections:
```bash
TODO
```

And add/modify the wlan0 static IPv4 and IPv6 sections:
```bash
TODO
```

### Chapter 0x04: Synchronizing the time with Network Time Servers
in progress, also subject to change

#### Why do we need network time servers?
Since your raspberry doesnt have a physical clock, its important to have it synchronize its clock with the configured network time servers available. Like anyone working with Cyber security, correct timestamps is of the essence in any evidence or logs. This to determine the correct timestamp something happened. Failing to do so, will render logs and evidence unusable.

#### Set the right expectations
Ok, you don't really have to change the *timesynccd* service, because it comes preinstalled on Debian Buster, however it is a good idea to have knowledge about this service existance, and that you don't need to install the ntp service. The NTP service which in turn will open port UDP/123, opening a footprint for traffic towards your Raspberry. The ntp service is complicated to secure, so unless you know how, don't use it.

#### Examples

Example: The location of your NTP (Network Time Protocol) server configuration
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
$ sudo systemctl status systemd-timesyncd
```
Example: Restart the **systemd-timesyncd** service.
```bash
$ sudo systemctl restart systemd-timesyncd
```
#### Troubleshooting
TODO

#### Summary
Thankyou for reading this section, this will help you in our later sections during logging, evidence and negotiating ipsec and prevent you from getting errors later when we use transport layer security with certificates. They will depend on you completing this section.

### Chapter 0x05: Installing and configuring components for RSyslog
in progress

### Chapter 0x06: Installing and configuring components for Wifi 802.11ac with WPA2-PSK
In this chapter we are going to install the hostapd, an essential component for making your Raspberry Pi 4 a wifi access point. This will enable the wlan0 adapter on the raspberry to act like a accesspoint. 

* **Advice** This chapter will only enable the hostapd and make it start broadcasting its ssid name.
* **Advice** Chapter 0x07, 0x08 are required to make the access point start moving traffic through it.
* **Advice** You will need to disable the wpa_supplicant.service

If you are using your Raspberry Pi 4, wifi to connect to another accesspoint, this will stop it from doing that. From now on your eth0 interface will be the outside/transit interface.

Lets get started

#### Installing and configuring the hostapd.service

Step 1: Update and upgrade your OS, then install the Wifi AccessPoint daemon.
```bash
$ sudo apt-get update
$ sudo apt-get upgrade -y
$ sudo apt-get install -y hostapd
```

Step 2: Make sure the hostapd.service is stopped.
```bash
$ sudo systemctl stop hostapd
```

Step 3: Start by disabling and masking the wpa_supplicant.service, which makes your raspberry a client to an external access point. This will stop the wifi client on your Raspberry Pi.
```bash
$ sudo systemclt stop wpa_supplicant.service
$ sudo systemctl disable wpa_supplicant.service
$ sudo systemctl mask wpa_supplicant.service
```

Step 4: And if you really want to be sure its not used, then..
```bash
$ sudo mv /etc/wpa_supplicant/wpa_supplicant.conf /etc/wpa_supplicant/wpa_supplicant.conf.old
```

Step 5: Make sure your *hostapd.conf* is pointed out inside the file */etc/default/hostapd*.
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
    # ** yes, enter it here.. even its deprecated **
    DAEMON_CONF="/etc/hostapd/hostapd.conf"

    # Additional daemon options to be appended to hostapd command:-
    #       -d   show more debug messages (-dd for even more)
    #       -K   include key data in debug messages
    #       -t   include timestamps in some debug messages
    #
    # Note that -B (daemon mode) and -P (pidfile) options are automatically
    # configured by the init.d script and must not be added to DAEMON_OPTS.
    #
    #DAEMON_OPTS=""

```

I have made a couple of hostapd configuration examples for you to select from. 
* **Advice** Please select the one that is most appropriate for you.
* **Advice** Read up on your documentation for your devices before selecting the one to choose.

#### Selecting a configuration

```bash
$ sudo nano /etc/hostapd/hostapd.conf
```

**Example configuration 1**: Wireless 802.11ac on 5Ghz, on channel 48, bandwidth 20/20Mhz. Recommended for iPhone 6 and upwards.
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
    
# enforce wpa2 algorithms. 1=wpa1, 2=wpa2, 3=combined wpa1 and wpa2 (not recommended)
wpa=2
    
# enforce use of pre-shared key.
wpa_key_mgmt=WPA-PSK
    
# enforce the CCMP encryption algorithm
rsn_pairwise=CCMP
    
# enforce hostapd with your preshared key.
wpa_passphrase=<enter your password here. 20-32 characters recommended.>
    
```

#### Enabling, starting, stopping and checking the hostapd.service

The hostapd is masked, which means you cannot enable it per default. Unmask it and then enable it.
```bash
$ sudo systemctl unmask hostapd
$ sudo systemctl enable hostapd
```

Example: Stop the hostapd.service
```bash
$ sudo systemctl stop hostapd.service
```

Example: Start the hostapd.service
```bash
$ sudo systemctl start hostapd.service
```

Example: Check status of the hostapd.service
```bash
$ sudo systemctl status hostapd.service
```

#### Verification
Step 1: Run iwconfig to see if your hostapd mode is now set to **Master**.
```bash
$ sudo iwconfig
```

```bash
    ...
    wlan0     IEEE 802.11  Mode:Master  Tx-Power=31 dBm   
              Retry short limit:7   RTS thr:off   Fragment thr:off
              Power Management:on
    ...
```
NOTE; The output should indicate that mode is *master* and that there is transmission power around *31dBm* with your built in wifi card, *wlan0*.

Step 2: Verify that wlan0 is acting as access point, AP, and that the channel you selected is active and working.
```bash
$ iw wlan0 info
```
```bash
Interface wlan0
	ifindex 3
	wdev 0x1
	addr xx:xx:xx:xx:xx:xx
	ssid wifi-03.firestorm.org
	type AP
	wiphy 0
	channel 48 (5240 MHz), width: 20 MHz, center1: 5240 MHz
	txpower 31.00 dBm
```

#### Troubleshooting

#### Summary
This chapter has set the foundation for your accesspoint configuration and for enabling the wifi 802.11 broadcasting. It should be broadcasting its ssid now, but lets finish the next chapter before connecting to the ssid.
