# Raspberry Pi connection on the go 
The new Raspberry Pi 4 has data transfer in the power USB-C and with OTG. You can use this to variant of thinks, but one I find cool, is to make a ethernet connection between your PC and the Raspberry Pi. This gives us a easy connection to our Pi's in the field.

## Setup on the Raspberry Pi
**Edit the interface configuration file:**
`
$sudo nano /etc/network/interfaces
`
Change to:
```
source /etc/network/interfaces.d/*
```

Create a new interface configuration 
```
$sudo nano /etc/network/interfaces.d/usb0.conf
```
Insert this configuration:
```
allow-hotplug usb0
iface usb0 inet static
    address 10.0.12.2/24
    gateway 10.0.12.1
    dns-nameservers 10.0.12.1
```
***If you wan't you can change this configuration. 'Address' is the Raspberry Pi's IP on this interface. 'Gateway' is your computers, you change this but remember to input this when you setup your PC later. 'Dns-nameservers' is also your PC. ***  

**Edit your DNS resolver file:**
```
$sudo nano /etc/resolv.conf
```
Insert to the end:
```
nameserver 10.0.12.1
```

**Edit the Kernel Command Line**
```
$sudo nano /boot/cmdline.txt
```
Add following after 'rootwait'
```
modules-load=dwc2,g_ether
```

**Edit the Raspberry Pi's configuration file**
```
$sudo nano /boot/config.txt
```
Add this to the end:
```
dtoverlay=dwc2
```

Reboot the Raspberry Pi. 
```
$sudo reboot now
```
If you list all the interfaces you should now see usb0
```
$iwconfig
```

## Windows Host Setup
To intergate with the Raspberry Pi over USB you need a driver.  
Install RNDIS gadget driver at: https://www.driverscape.com/download/usb-ethernet-rndis-gadget

When you have installed the driver, plugged in your Raspberry Pi, you should see a connection under controlepanel/networkconnections named RNDIS. Right click on it and go to properties. Click on 'TCP/IPv4...' and properties. Input your IP configuration:  
    Ip address: 10.0.12.1  
    Subnet mask: 255.255.255.0  
    Default gateway: 10.0.12.1  

You can use Google's DNS server:  
8.8.8.8
