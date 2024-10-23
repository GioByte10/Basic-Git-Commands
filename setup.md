## Updating Packages
First we'll want to update system packages to the latest version. This is a two step process. First we want to update the package index, which basically lets the Pi know what is the latest version of the installed packages.
```
sudo apt update
```
Once the Pi has the updated package index, we can ahead and install their latest version
```
sudo apt upgrade
```


## Pi configuration
We can configure the Pi through the `Raspberry Pi Software Configuration Tool`, which we can access through
```
sudo raspi-config
```
Here we have multiple settings we can play with, but the ones we'll be changing are the following:
- System Options/Boot / Auto Login &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; → console autologin
- Interface Options/ SSH &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; → enable
- Localisation Options &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; → timezone, keyboard, etc


## Networks
Scan for WiFi networks
```
nmcli d wifi list ifname wlan0
```

In order to connect the Pi to a WiFi network we need to modify the `wpa_supplicant.conf` file. We can open it on nano using:
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Then restart the DHCP client
```
sudo dhcleint wlan0
```

Check network configuration
```
ifconfig wlan0
```

Check network status
```
iwconfig wlan0
```

We can further check if the Pi is connected to internet by pinging a website, e.g.,
```
ping www.google.com
```

## Miscellaneous
Check disk usage
```
df -h
```

## Basic Linux Commands

`sudo command` run `command` with admin priviliges
`ls` list the contents of the directory<br>
`cd directory` navigate to `directory`<br>
`~` home directory<br>
`\` root directory<br>
`mkdir directory` make `directory`<br>
`rmdir directory` delete empty `directory`<br>
`rm -r directory` delete non-empty `directory`<br>
`rm file` delete `file`<br>


## Troubleshooting
P: I connected through `sudo raspi-config` and can't seem to find the network information in the supplicant file.<br>
A: The network was added under `/etc/NetworkManager/system-connections`. Delete the file and add the information in the `wpa_supplicant.conf`
