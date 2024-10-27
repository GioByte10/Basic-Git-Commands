## Pi configuration
We can configure the Pi through the `Raspberry Pi Software Configuration Tool`, which we can access through
```
sudo raspi-config
```
Here we have multiple settings we can play with, but the ones we'll be changing are the following:
- System Options/Wireless LAN &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; → connect to a personal network (hotspot)
- System Options/Boot / Auto Login &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; → console autologin
- Interface Options/ SSH &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; → enable
- Localisation Options &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; → timezone, keyboard, etc


## Updating Packages
First we'll want to update system packages to the latest version. This is a two step process. First we want to update the package index, which basically lets the Pi know what is the latest version of the installed packages.
```
sudo apt update
```
Once the Pi has the updated package index, we can ahead and install their latest version
```
sudo apt upgrade
```

## Setup WiFi

Check if dhcpcd is installed
```
which dhcpcd
```

If not installed
```
sudo apt install dhcpcd5
```

Update `dhcpcd.conf` by adding the line `interface wlan0` at the end of the file
```
sudo nano /etc/dhcpcd.conf
```

Enable and start dhcpcd
```
sudo systemctl enable dhcpcd
sudo systemctl start dhcpcd
```

Scan for WiFi networks
```
sudo iwlist wlan0 scan | grep "ESSID"
```

In order to connect the Pi to a WiFi network we need to modify the `wpa_supplicant.conf` file. We can open it on nano using:
```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Add the network configuration
```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

# for personal/home networks
network={
    ssid="Your_SSID"
    psk="Your_Password"
    priority=1
}

# for UCSD network
network={
    ssid="UCSD-PROTECTED"
    key_mgmt=WPA-EAP
    eap=PEAP
    identity="UCSD_Username"
    password="UCSD_Password"
    phase2="auth=MSCHAPV2"
    priority=2
}
```


Stop the network manager since it conflicts with the supplicant file
```
sudo systemctl stop NetworkManager
sudo systemctl disable NetworkManager
```

Enable and start wpa_supplicant
```
sudo systemctl enable wpa_supplicant
sudo systemctl start wpa_supplicant
```

Reboot the Pi
```
reboot
```

Check network status
```
iwconfig wlan0
```

We can further check if the Pi is connected to internet by pinging a website, e.g.,
```
ping www.google.com
```
<br><br>
If you were successful, stop here, else follow the these extra steps. Go into the wpa_supplicant service
```
sudo nano /etc/systemd/system/wpa_supplicant@wlan0.service
```

Add the following:
```
[Unit]
Description=WPA supplicant for %i
Requires=sys-subsystem-net-devices-%i.device
After=sys-subsystem-net-devices-%i.device

[Service]
ExecStart=/sbin/wpa_supplicant -c /etc/wpa_supplicant/wpa_supplicant.conf -i %i
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start the service
```
sudo systemctl enable wpa_supplicant@wlan0.service
sudo systemctl start wpa_supplicant@wlan0.service
```

Check the process' status
```
sudo systemctl status wpa_supplicant@wlan0.service
```

## Miscellaneous
Check disk usage
```
df -h
```

# Apendix
Other useful commands

## WiFi
Enable WiFi
```
sudo raspi-config nonint do_wifi_country US
```

Check if WiFi is blocked
```
sudo rfkill list
```

Unblock soft blocked WiFi
```
sudo rfkill unblock wifi
```

Update supplicant process without rebooting
```
sudo wpa_cli -i wlan0 reconfigure
```

Manually connect to WiFi
```
sudo wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```

Manually connect to WiFi 2.0
```
sudo wpa_cli -i wlan0 reconfigure
sudo dhclient wlan0
```

Check `wpa_supplicant` logs
```
journalctl -u wpa_supplicant
journalctl -xe | grep wpa
```

Set correct permissions to supplicant file
```
sudo chmod 600 /etc/wpa_supplicant/wpa_supplicant.conf
```

Reset WiFi interface
```
sudo ifconfig wlan0 down
sudo ifconfig wlan0 up
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
`ctrl + c` exit pretty much anything

## Advanced Linux Commands

`sudo systemctl action process` perform `action` (enable, disable, start, stop, etc) on `process`
`ps aux | grep process` dump `process` status/data


## Troubleshooting
P: I connected through `sudo raspi-config` and can't seem to find the network information in the supplicant file.<br>
A: The network was added under `/etc/NetworkManager/system-connections`. Delete the file and add the information in the `wpa_supplicant.conf`
