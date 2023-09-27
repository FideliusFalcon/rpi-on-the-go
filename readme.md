# Raspberry Pi connection on the go 
The Raspberry Pi 4 has data transfer in the power USB-C and with OTG. You can use this to variant of thinks, but one I find cool, is to make a ethernet connection between your PC and the Raspberry Pi. This gives us a easy connection to our Pi's in the field - This is also possible with the USB port on the Raspberry Pi Zero

## Setup on the Raspberry Pi
**Check that the interface configuration file is correct (`/etc/network/interfaces`)**
There should be an entry with following:
```
source /etc/network/interfaces.d/*
```
Create a new interface configuration 
```
sudo nano /etc/network/interfaces.d/usb0.conf
```

Insert this configuration:
```
allow-hotplug usb0
iface usb0 inet static
    address 10.0.5.2/24
```
*address is the IP address that the Raspberry Pi will get*  


**Edit the Kernel Command Line**
```
sudo nano /boot/cmdline.txt
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
sudo reboot now
```

If you list all the interfaces you should now see usb0
```
iwconfig
```

## Windows Host Setup
To intergate with the Raspberry Pi over USB you need a driver.  
Install RNDIS gadget driver at: https://www.driverscape.com/download/usb-ethernet-rndis-gadget

When you have installed the driver, plugged in your Raspberry Pi, you should see a connection under controlepanel/networkconnections named RNDIS/Ethernet Gadget. Right click on it and go to properties. Click on 'TCP/IPv4...' and properties. Input your IP configuration:  
```
    Ip address: 10.0.5.1  
    Subnet mask: 255.255.255.0  
```

You can use CloudFlare's DNS server:  
1.1.1.1
