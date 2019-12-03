# Raspberry Pi connection on the go 
The new Raspberry Pi 4 has data transfer in the power USB-C and with OTG. You can use this to variant of thinks, but one I find cool, is to make a ethernet connection between your PC and the Raspberry Pi. This gives us a easy connection to our Pi's in the field.

## Setup on the Raspberry Pi
Edit the interface configuration file:
'''
$sudo nano /etc/network/interfaces
'''
Change to:
'''
source /etc/network/interfaces.d/*
'''

Create a new interface configuration 
'''
$sudo nano /etc/network/interfaces.d/usb0.conf
'''

'''
allow-hotplug usb0
iface usb0 inet static
    address 10.0.12.2/24
    gateway 10.0.12.1
    dns-nameservers 10.0.12.1
'''
If you wan't you can change the configuration. 'Address' is the Raspberry Pi's IP on this interface. 'Gateway' is your computers, you change this but remember to input this when you setup your PC later. 'Dns-nameservers' is also your PC.   